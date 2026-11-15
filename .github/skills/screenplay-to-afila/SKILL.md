---
name: screenplay-to-afila
description: Break a screenplay, script or story into Afila primitives (characters, stills, scenes) written as `.afila/**/*.yaml`. Use when the user supplies a screenplay/story/treatment and wants it turned into Afila assets or a video, or asks how to structure a story for Afila.
---

# Screenplay → Afila primitives

Turn long-form writing into the YAML files the Afila editor generates assets from. Output goes in `.afila/` of the user's repo. Call `afila_get_authoring_guide` for the full rules, and always finish by calling the `afila_validate_project` MCP tool (pass the files' contents) and fixing every error.

## Primitives

| Primitive | Folder | Purpose |
|---|---|---|
| Character | `.afila/characters/<name>.yaml` | One portrait per recurring character; the visual identity anchor. |
| Still | `.afila/stills/<name>.yaml` | Establishing locations, key props, and keyframe images. |
| Scene | `.afila/scenes/<name>.yaml` | A 3–30 s video clip made of shots, with audio generated. |

Generation order is characters → stills → scenes. Scenes cite characters/stills with `@name` (case-insensitive) and the generated image is used as the reference.

## Hard rules (the parser rejects violations)

- `metadata.name`: only `A-Z a-z 0-9 _ -`, ≤30 chars, unique across characters+stills (they share the `@` namespace). Use kebab-case; match the filename.
- Scene length: whole seconds, 3–30. When `shots` exist, the total is the **sum of shot durations** (keep `options.duration` equal to it). Over 15 s is auto-extended.
- Per scene: ≤6 distinct reference images (every `@name` in prompts + keyframes).
- `options.aspectRatio`: `16:9` or `9:16`. `options.resolution`: `360p|720p|1080p|4k`.
- `keyframes`: only `first` and/or `last` (names of characters/stills). With `last`, scene must be ≤10 s. No `middle`.
- `spec.continuesFrom: <scene-name>`: seeds the scene's opening frame from the **last frame of the rendered previous scene** (continuous action, same location). The target must be another scene, no cycles, and it cannot be combined with `keyframes.first`. It uses one of the 6 image slots. The target must be generated first; if it fails, the dependent scene is skipped.
- Scene order = alphabetical by filename (a `continuesFrom` target is always processed first). Use zero-padded names: `s001-…`, `s002-…`.
- Do NOT create `.afila/voices/` or `spec.voices` and do not use `enableAudio` (all removed; audio is always generated).
- Never write `status` blocks with `success: true`/`reference` yourself. New assets use `status:\n  success: false`.
- Do not name underlying AI models in user-facing text.

## Breakdown procedure

1. **Read the whole text first.** List characters (recurring, speaking, visually distinct), locations, and beats.
2. **Pick a visual style line** (e.g. "ligne claire cartoon style, clean bold outlines, flat cel-shaded colors") and append it to *every* character and still prompt and to scene `dataPrompt`s. Consistency of style is what makes clips cut together.
3. **Characters**: one file per recurring character, ≤6 per scene. Describe age, hair, clothing (fixed outfit), expression, style. Put "blurry, low quality, deformed" (+ style opposites) in `dataPromptNegative`. Minor extras don't need files; describe them inline in prompts.
4. **Stills**: one per key location (`@Characters` can be mentioned inside still prompts to bake them in), plus one per keyframe needed for continuity. Set `options.outputFormat` (`png`) and `aspectRatio`.
5. **Scenes**: split the screenplay on sluglines/beat changes into clips of ~5–15 s. Each scene = 1–6 shots of 3–10 s. Longer sequences become several scenes named in order (`s001-arrival`, `s002-chase`) — see **Continuity** and **Large screenplays** below.
6. **Shot prompt format** (matches the samples):
   `Shot N (Ds): <camera>, <action with @refs>. @hero says "<dialogue>". SFX: <sounds>. No background music.`
   - Dialogue in double quotes with voice direction ("bright young voice").
   - Put the scene-level look/setting in `spec.dataPrompt`; put things to avoid in `dataPromptNegative` (e.g. `dark, scary, blurry, low quality, text overlays, watermark`).
   - Shot `dataPromptNegative` may be `''`.
7. **Budget**: credits = characters 1, stills 2, scenes `ceil(seconds × rate/5 + 0.5)` with rate/5s 360p=2, 720p=5, 1080p=8, 4k=15. Default to `720p`; offer a 360p draft pass first. Run `afila_validate_project` for an exact estimate and tell the user before generating.
8. **Safety**: screenplays with graphic violence/sexual content get neutralized by the service; soften or imply instead of depicting.
9. Write a short `STORYBOARD.md` (scene → source page/slugline, characters, est. credits) so the user can trace output back to the script.

## File templates

```yaml
# .afila/characters/maya.yaml
type: character
metadata:
  name: maya
spec:
  dataPrompt: A 30-year-old lighthouse keeper with a grey braid, yellow raincoat, weathered kind face, <STYLE>
  dataPromptNegative: blurry, low quality, deformed
status:
  success: false
```

```yaml
# .afila/stills/lighthouse-dawn.yaml
type: still
metadata:
  name: lighthouse-dawn
spec:
  dataPrompt: A white lighthouse on a cliff at dawn, calm sea, @maya on the balcony, <STYLE>
  dataPromptNegative: blurry, low quality, realistic, dark
  options:
    outputFormat: png
    aspectRatio: '16:9'
status:
  success: false
```

```yaml
# .afila/scenes/s001-dawn.yaml
type: scene
metadata:
  name: s001-dawn
spec:
  dataPrompt: |
    @maya at the lighthouse balcony at dawn, <STYLE>
  dataPromptNegative: dark, scary, blurry, low quality, text overlays, watermark
  shots:
    - name: establishing
      dataPrompt: |
        Shot 1 (5s): Wide shot, sun rises over the sea behind @lighthouse-dawn. SFX: waves, gulls. No background music.
      dataPromptNegative: ''
      duration: 5
    - name: line
      dataPrompt: |
        Shot 2 (5s): Close-up, @maya says softly "Another day, another storm that never came." SFX: wind. No background music.
      dataPromptNegative: ''
      duration: 5
  keyframes:
    first: lighthouse-dawn
  options:
    duration: 10
    aspectRatio: '16:9'
    resolution: 720p
status:
  success: false
```

## Continuity (critical for long sequences)

Every scene is rendered independently, so continuity must be designed. Per boundary between scene N and N+1 choose:

| Situation | Use | Notes |
|---|---|---|
| Same location/time, action carries on | `continuesFrom: <N>` on scene N+1 | Real rendered end-state is the opening frame; scene can be up to 15 s; dialogue fine. Still image only — re-describe motion, lighting and positions in the first shot. |
| Hard cut, new location, or time jump | New location still as `keyframes.first`; optionally `keyframes.last` on N matching a shared still | Keyframe scenes must be ≤10 s; the still must be generated before the scene. |
| Same character, new angle in the same room | Nothing special; mention the same `@refs` and location still | Cheapest. |

Rules that keep long sequences coherent:
- **One STYLE line** in every prompt; never vary it.
- **Re-anchor each shot**: only the last ~10 s of context influence a scene, so name the character (`@maya`), her outfit and the location in every shot, not just the first.
- **Wardrobe/time variants are separate characters/stills**: `maya`, `maya-storm`, `lighthouse-dawn`, `lighthouse-night`. Pick the variant by the ledger, never mid-scene.
- **Screen direction & positions**: keep a ledger (below) of who is left/right and what they hold, and state it in the first shot of the next scene.
- **6-image budget**: character + still + continuation seed all count. Prefer ≤2 characters + 1 location still + the seed; fold props into the prompt text.
- Don't chain `continuesFrom` through more than ~5 scenes without a still anchor; drift accumulates. Insert a location still as `keyframes.first` at the next hard cut to reset it.

## Large screenplays

1. **Story bible first** (`BIBLE.md`): STYLE line, a card per character (look, fixed outfit, variants), a location list with time-of-day variants, recurring props. Create all characters and location stills from this before any scene.
2. **Continuity ledger** in `STORYBOARD.md`, one row per scene: `scene | slugline | characters (variant) | location still | time | entry state | exit state | screen direction | link (continuesFrom X / keyframe Y / hard cut)`. Fill it before writing scene YAML, and re-read the previous row when writing the next scene.
3. **Group scenes into sequences** (~5–15 scenes, each sequence one location/act). Stay far below the validate limits (200 files, 200 KB per file).
4. **Queue, don't dump**: one job generates *everything* not yet successful. Put the future sequences' scene YAMLs in `.afila-queue/seq02/`, `.afila-queue/seq03/` (outside `.afila/`) and move a batch into `.afila/` only when ready. Characters and location stills for the whole script can go in batch 0. The `afila-generate` skill runs this loop, including merging each result branch back before the next batch.
5. **Dependencies before dependents**: a `continuesFrom` target must be `success: true` (generated and merged) before its dependent scene enters `.afila/`; or put both in the same batch (processed in order).
6. Run `afila_validate_project` per batch with all files currently in `.afila/`, and spot-check images from the previous batch before continuing.

See `examples/feature-length-outline/` for a three-sequence example with a ledger and queue, and `examples/screenplay-demo/` for a full worked example. Next step after validation: use the `afila-generate` skill.
