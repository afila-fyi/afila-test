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

1. Build the MCP server once: `cd ../../../afila/mcp && npm ci && npm run build`. The repo's `.vscode/mcp.json` / `.mcp.json` register it as `afila`; set `AFILA_API_URL`.
2. Ask the agent:
   > Use screenplay-to-afila on `examples/screenplay-demo/screenplay.md`, validate it, then use afila-generate to render it on branch `afila/lighthouse`.
3. The agent (a) writes/updates the YAML (already provided here as the expected result), (b) calls `afila_validate_project`, (c) `afila_get_balance`, (d) commits and pushes after your confirmation, (e) `afila_submit_job` and `afila_wait_for_job`, (f) pulls the results and lists each asset's `status.reference` URL.

Validate locally at any time:

```bash
cd ../../../afila/mcp && npm run check -- "$(realpath ../../afila-test/examples/screenplay-demo)"
```

All assets start with `status.success: false`; after a job the editor commits `status.reference` URLs back.
