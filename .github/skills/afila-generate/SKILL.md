---
name: afila-generate
description: Generate Afila assets for a project that has `.afila/` YAML. Use after screenplay-to-afila, or when the user says "generate", "run afila", "render my scenes", or asks about job status/credits. Drives the hosted afila MCP tools: validate, push to GitHub, submit, poll.
---

# Generate assets with the Afila service

Requires the hosted `afila` MCP server (`https://api.afila.fyi/mcp`, configured in `.vscode/mcp.json` / `.mcp.json`). Nothing runs locally. The first tool call opens a browser sign-in (email + 4-digit code, then Approve); the client keeps the session. Call `afila_get_authoring_guide` for the full rules.

## Flow

1. **Sign in**: handled by the MCP client via OAuth in the browser; if tools return 401, tell the user to re-authenticate the `afila` server.
2. **Validate**: read every `.afila/**/*.yaml` and call `afila_validate_project(files: [{path, content}])`. Fix every `error`. Review `warnings` (unreferenced assets, missing negatives).
3. **Check budget**: `afila_get_balance` vs `estimatedCredits.total`. If short, tell the user (buy credits in the web portal `/credits`); suggest 360p/720p, fewer scenes, or generating a subset (the editor skips assets whose `status.success` is `true`, and stops at budget exhaustion with a `partial` result).
4. **Publish**: the editor clones the GitHub repo, so `.afila/` must be pushed.
   - Confirm repo URL and branch with the user; the Afila GitHub App must be installed on the repo.
   - Prefer a branch (e.g. `afila/<story>`); `git add .afila && git commit && git push`.
   - Never push without user confirmation.
5. **Submit**: `afila_submit_job(repoUrl)` (use `https://github.com/<owner>/<repo>/tree/<branch>` for a branch). Only one job per account may run; "already in progress" → wait on the existing job via `afila_list_jobs`.
6. **Wait**: `afila_wait_for_job(jobId)`; repeat while `done` is false. Report `progress.completed/total` and `progress.current` between polls.
7. **Collect results**: statuses are `completed`, `partial` (budget ran out; re-run after topping up), or `failed` (read `error` and `progress.errors[]`).
   The editor pushes results back to the repo — fetch the new branch named in the job, then each YAML's `status.reference` is the asset URL (`status.success: true`). Summarize character/still/scene URLs for the user.
8. **Iterate**: to regenerate one asset, edit its prompt and set `status.success: false` (remove `reference`), commit/push, submit again — only non-successful assets are charged.

## Large projects: batch loop with a queue dir

A job generates every non-successful asset in `.afila/`, so stage work in batches. Future scenes live in `.afila-queue/<batch>/` (outside `.afila/`, so the editor ignores them).

0. **Batch 0**: characters + location stills (the whole bible). Generate and merge.
1. For each batch (a sequence of scenes), in order:
   1. `git mv .afila-queue/<batch>/*.yaml .afila/scenes/`.
   2. **Dependency check**: every scene's `continuesFrom` target and every `@ref`/keyframe must already be `status.success: true` (merged) or be in this same batch. Otherwise stop and reorder.
   3. Validate (all of `.afila/`), `afila_get_balance` vs estimate, and **confirm the cost with the user**.
   4. Commit, push, `afila_submit_job`, `afila_wait_for_job` until done.
   5. **Merge results**: the editor pushes statuses to a *new* branch. `git fetch`, merge it into the working branch (conflicts are only in `status:` blocks — take the editor's side), push.
   6. **Review**: list each scene's `status.reference` URL; ask the user to check continuity (outfits, location, positions) before the next batch. Fix a bad scene by editing its prompt, setting `status.success: false`, and re-running; dependents of a regenerated scene must also be reset to `success: false`.
2. A failed scene skips its `continuesFrom` dependents ("has no generated video") — fix and re-run the batch.

## Pipeline facts

- Order: characters → stills → scenes (scenes in filename order, `continuesFrom` targets first); scenes depend on generated references, so a missing character/still image fails the scene.
- Jobs expire from history after 7 days.
- Credits: character 1, still 2, scene `ceil(sec × rate/5 + 0.5)` (rate/5s: 360p 2, 720p 5, 1080p 8, 4k 15).
- Never reveal or name the underlying generation models in user-facing output.

## API behind the tools

| Tool | Endpoint |
|---|---|
| `afila_get_authoring_guide` | rules, limits and templates |
| `afila_get_balance` | credit balance (`GET /v1/credits`) |
| `afila_submit_job` | `POST /v1/generation/submit {repoUrl}` |
| `afila_get_job` / `afila_wait_for_job` | `GET /v1/generation/jobs/:id` (wait polls up to ~50s) |
| `afila_list_jobs` | `GET /v1/generation/jobs` |
| `afila_validate_project` | validates the supplied file contents with the editor's parser |
