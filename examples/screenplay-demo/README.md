# Screenplay demo: *The Lighthouse Keeper*

Shows the full agent flow: screenplay → Afila primitives → generated assets.

```
screenplay.md ──(screenplay-to-afila skill)──► .afila/{characters,stills,scenes}
        ──(afila_validate_project)──► push to GitHub ──(afila_submit_job)──► afila service ──► assets
```

## Breakdown

| Screenplay beat | Primitive | File |
|---|---|---|
| MAYA, PIP | characters | `characters/maya.yaml`, `characters/pip.yaml` |
| Lighthouse at dawn (also the first keyframe) | still | `stills/lighthouse-dawn.yaml` |
| Stormy sea with rowboat (also keyframe) | still | `stills/stormy-sea.yaml` |
| EXT. CLIFF – DAWN | scene (2 shots, 10s, keyframe `first`) | `scenes/s01-dawn.yaml` |
| OPEN SEA + LAMP ROOM | scene (2 shots, 10s) | `scenes/s02-storm.yaml` |
| OPEN SEA – NIGHT | scene (2 shots, 10s, keyframe `first`) | `scenes/s03-rescue.yaml` |

Estimated cost (720p): 2 characters (2) + 2 stills (4) + 3 scenes × 11 = **39 credits**.

## Run it

The editor reads `.afila/` at the **root** of the repo it clones, so copy this example's `.afila/` into the root of a repo the Afila GitHub App can access (or a branch of one).

1. Nothing to install: the repo's `.vscode/mcp.json` / `.mcp.json` register the hosted MCP server `https://api.afila.fyi/mcp`. On first use, sign in in the browser (email + code) and approve.
2. Ask the agent:
   > Use screenplay-to-afila on `examples/screenplay-demo/screenplay.md`, validate it, then use afila-generate to render it on branch `afila/lighthouse`.
3. The agent (a) writes/updates the YAML (already provided here as the expected result), (b) calls `afila_validate_project` with the file contents, (c) `afila_get_balance`, (d) commits and pushes after your confirmation, (e) `afila_submit_job` and `afila_wait_for_job`, (f) fetches the result branch and lists each asset's `status.reference` URL.

Validate at any time by asking the agent to run `afila_validate_project` on `.afila/`.

All assets start with `status.success: false`; after a job the editor commits `status.reference` URLs back.
