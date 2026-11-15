# Feature-length outline: continuity across sequences

A miniature of how to take a **long** screenplay through Afila. Same lighthouse world as `screenplay-demo`, but organised for scale:

- `.afila/` — batch 0 (all characters + location stills) and sequence 1 (scenes ready to generate now).
- `.afila-queue/seq02`, `seq03` — later sequences, parked outside `.afila/` so the editor ignores them until the agent moves them in (see the `afila-generate` skill's batch loop).
- `STORYBOARD.md` — the continuity ledger.

## Continuity techniques shown

| Boundary | Technique |
|---|---|
| `s001` → `s002` | `continuesFrom: s001-dawn` — same balcony, same light; opening frame is the last rendered frame of `s001`. |
| `s002` → `s003` | Hard cut to the night lamp room: `keyframes.first: lighthouse-night`, outfit switches to the `maya-night` character. |
| `s003` → `s004` | `continuesFrom: s003-lamp-room`. |
| `s004` → `s005` | Hard cut back to dawn with the `maya` (raincoat) variant and the `lighthouse-dawn` still as first frame. |

## Run it

1. Validate `.afila/` with `afila_validate_project`; generate batch 0 + sequence 1.
2. Merge the result branch back, check the URLs.
3. `git mv .afila-queue/seq02/*.yaml .afila/scenes/` (its dependencies are already generated), repeat.

Scenes use 360p to keep the whole outline cheap. Raise `options.resolution` for final renders.
