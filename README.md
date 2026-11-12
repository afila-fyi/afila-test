# afila-test

Sample Afila project. Every file in `.afila/` is a YAML asset. Scenes are generated with Google Gemini Omni (`gemini-omni-1.2-flash`).

Scene credit cost is `ceil(seconds × rate / 5 + 0.5)`. Rates per 5s: 360p = 2, 720p = 5, 1080p = 8, 4k = 15. Each scene adds 0.5.

## Scene samples

| Scene | Demonstrates | Options | Credits |
|---|---|---|---|
| `greeting` | Minimal single shot | 16:9, 720p, 5s | 6 |
| `park-meeting` | Two shots, dialogue and SFX | 16:9, 1080p, 10s | 17 |
| `tropical-voyage` | Three shots, negative prompt | 16:9, 1080p, 15s | 25 |
| `vertical-story` | Vertical 9:16 for phones | 9:16, 720p, 10s | 11 |
| `draft-preview` | Cheap 360p preview | 16:9, 360p, 3s | 2 |
| `cinematic-4k` | 4k output (upscaled) | 16:9, 4k, 5s | 16 |
| `epic-voyage-30s` | 30s scene, auto-chained as 15s + 10s + 5s extension turns | 16:9, 720p, 30s | 31 |
| `keyframe-transition` | First and last keyframes (interpolation, max 10s) | 16:9, 720p, 8s | 9 |
| `from-still` | First keyframe only, animating a still | 16:9, 1080p, 8s | 14 |
| `ensemble-six` | Six `@` references (limit) | 16:9, 720p, 10s | 11 |
| `dialogue-and-audio` | Spoken lines and voice direction in the prompt, SFX, no music | 16:9, 720p, 10s | 11 |

## Stills

`park-adventure`, `hero-and-sidekick-park` (16:9 png), `keyframe-start`, `keyframe-end` (keyframe sources, 16:9 png), `portrait-hero` (9:16 webp).

## Limits worth knowing

- Aspect ratios: 16:9 and 9:16 only. Resolutions: 360p, 720p, 1080p, 4k.
- Scene length 3–30s. Over 15s uses extension turns. With first and last keyframes the max is 10s.
- At most 6 reference images per scene (characters, stills and keyframes).
- No custom voices or `.afila/voices`: describe the voice in the prompt.
- Keyframes support `first` and `last` only (no `middle`).
- Use `dataPromptNegative` for things to avoid.
