# afila-test

Sample Afila project. Every file in `.afila/` is a YAML asset. Pipeline order: characters → stills → scenes. `status.reference` holds the generated asset's URL; `success: false` means not generated yet.

For an end-to-end example that turns a screenplay into these files, see [`examples/screenplay-demo`](examples/screenplay-demo/README.md).

Scene credit cost is `ceil(seconds × rate / 5 + 0.5)`. Rates per 5s: 480p = 3, 720p = 6, 1080p = 12. Each scene adds 0.5.

## Scene samples

| Scene | Demonstrates | Options | Credits |
|---|---|---|---|
| `greeting` | Minimal single shot | 16:9, 720p, 5s | 7 |
| `park-meeting` | Two shots, dialogue and SFX | 16:9, 1080p, 10s | 25 |
| `tropical-voyage` | Three shots, negative prompt | 16:9, 1080p, 15s | 37 |
| `vertical-story` | Vertical 9:16 for phones | 9:16, 720p, 10s | 13 |
| `draft-preview` | Cheap 480p preview | 16:9, 480p, 3s | 3 |
| `epic-voyage-30s` | 30s scene in a single generation | 16:9, 720p, 30s | 37 |
| `keyframe-transition` | First and last keyframes (interpolation; no @references) | 16:9, 720p, 8s | 11 |
| `from-still` | First keyframe only, animating a still | 16:9, 1080p, 8s | 20 |
| `ensemble-six` | Six `@` references | 16:9, 720p, 10s | 13 |
| `dialogue-and-audio` | Spoken lines and voice direction in the prompt, SFX, no music | 16:9, 720p, 10s | 13 |

## Stills

`park-adventure`, `hero-and-sidekick-park` (16:9 png), `keyframe-start`, `keyframe-end` (keyframe sources, 16:9 png), `portrait-hero` (9:16 webp).

## Limits worth knowing

- Aspect ratios: 16:9, 9:16, 4:3, 3:4, 1:1, 21:9. Resolutions: 480p, 720p, 1080p.
- Scene length 2–30s, generated in one pass. A last keyframe switches to strict first/last-frame mode, which cannot be combined with `@` references.
- At most 10 reference images per scene (characters, stills and keyframes).
- `spec.continuesFrom: <scene>` opens a scene with the last frame of an earlier rendered scene (counts toward the 10 images; cannot be combined with `keyframes.first`; the target must be generated first).
- No custom voices or `.afila/voices`: describe the voice in the prompt.
- Keyframes support `first` and `last` only (no `middle`).
- Use `dataPromptNegative` for things to avoid.

## Agent workflow (screenplay → assets)

- Install the **afila plugin** (skills + MCP server in one): `copilot plugin marketplace add afila-fyi/afila` then `copilot plugin install afila@afila` (Claude Code: `/plugin marketplace add afila-fyi/afila`, `/plugin install afila@afila`). Skills it provides: `screenplay-to-afila` (break a screenplay/story into characters, stills and scenes) and `afila-generate` (validate, push, submit, poll).
- Hosted remote MCP server `afila` (`https://api.afila.fyi/mcp`, OAuth browser sign-in, nothing to run locally), registered in `.vscode/mcp.json` and `.mcp.json`. Tools: `afila_get_authoring_guide`, `afila_get_balance`, `afila_validate_project`, `afila_submit_job`, `afila_get_job`, `afila_list_jobs`, `afila_wait_for_job`.
- Worked example: [`examples/screenplay-demo`](examples/screenplay-demo/README.md).
- Long scripts and scene-to-scene continuity (`spec.continuesFrom`, keyframe chaining, ledger, batch queue): [`examples/short-film-the-signal`](examples/short-film-the-signal/README.md).
