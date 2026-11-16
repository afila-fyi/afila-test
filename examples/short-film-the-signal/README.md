# Short film: The Signal

A ~2-minute, 13-scene film showing how to keep **continuity, consistent characters, consistent voices and film-like flow** across a long script.

| File | Purpose |
|---|---|
| `screenplay.md` | Input script |
| `BIBLE.md` | Style line, character descriptors, voice bible, ambience, film grammar |
| `STORYBOARD.md` | Continuity ledger (one row per scene) |
| `.afila/` | Batch 0 (3 characters + 4 stills) and sequence 1 (3 scenes) |
| `.afila-queue/seq02`, `seq03` | Later sequences, moved into `.afila/` one batch at a time |

## Techniques

- **Continuous action** → `continuesFrom` (s002←s001, s003←s002, s005←s004, s006←s005, s008←s007, s012←s011, s013←s012).
- **Hard cuts / cross-cuts / time jump** → location still as `keyframes.first`.
- **Same face, same clothes** → character descriptors never change; `maya` / `maya-night` only differ at the scripted time jump.
- **Same voice** → the voice phrases in `BIBLE.md` are pasted verbatim before every line (radio lines add the static phrase).
- **Flow** → establishing → medium → close-up; each continuing scene starts "Continuing directly from the previous shot…" and re-names outfit and location; ambience phrases repeat across cuts.

## Run it (≈ 3 characters + 4 stills + 13 scenes at 480p)

1. Ask the agent: *"Use the afila plugin to generate this film"* (or run each step yourself).
2. Batch 0 + sequence 1: validate `.afila/`, push, generate, merge the result branch, review.
3. `git mv .afila-queue/seq02/*.yaml .afila/scenes/`, repeat for `seq03`. Every `continuesFrom` target is either already generated or in the same batch.
4. Raise `options.resolution` to `720p` or higher for final renders.
