# Short film: The Golden Arrow (Robin Hood)

A ~50-second, 6-scene film in one batch. Compact showcase of **narration, quoted dialogue with consistent voices, SFX/ambience, `continuesFrom` and a hard cut** using the public-domain Robin Hood folk tale with original character designs.

| File | Purpose |
|---|---|
| `screenplay.md` | Input script |
| `BIBLE.md` | Style line, character descriptors, voice bible, ambience |
| `STORYBOARD.md` | Continuity ledger |
| `.afila/` | 3 characters, 2 stills, 6 scenes, 1 film (all ungenerated) |

## Techniques

- **Continuous action** → `continuesFrom` (s002←s001, s003←s002, s005←s004, s006←s005).
- **Hard cut** (bridge → tournament) → `keyframes.first: archery-field` on s004.
- **Opening shot** → `keyframes.first: sherwood-bridge` on s001. Keyframe scenes do not mix with `continuesFrom`.
- **Same voices** → voice phrases from `BIBLE.md` pasted before each line; narrator is "off-screen" and never shown.
- **One finished video** → `.afila/films/robin-hood.yaml` stitches the six scenes in order (free): cuts between continuous scenes, a 0.5s crossfade at the hard cut into the tournament, and fades at both ends. About 75s in total.
- **Audio** → every shot lists `SFX:` cues and ends with `No background music.`

## Cost (480p)

3 characters (1 each) + 2 stills (2 each) + scenes (`ceil(sec × 3/5 + 0.5)`): 6 + 7 + 6 + 6 + 5 + 6 = 36 → **43 credits**. The film adds no credits. Use `afila_validate_project` for an exact estimate.

## Run it

1. Ask the agent: *"Use the afila plugin to generate this film"*, or push `.afila/` to a repo with the Afila GitHub App installed and submit a job.
2. Generate everything in one job: characters and stills first, then scenes in filename order, so each `continuesFrom` target is ready before its dependent scene.
3. Raise `options.resolution` to `720p` or `1080p` for final renders.
