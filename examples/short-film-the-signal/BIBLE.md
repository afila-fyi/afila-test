# The Signal: story bible

**STYLE (used verbatim in every prompt):** ligne claire cartoon style, clean bold outlines, flat cel-shaded colors

## Characters (fixed descriptors, reused verbatim)

| Name | Look | Wardrobe variants |
|---|---|---|
| `maya` | 30-year-old lighthouse keeper, long grey braid over her left shoulder, brown eyes, weathered kind face | `maya` = bright yellow raincoat (dawn); `maya-night` = dark blue wool sweater (night, indoors) |
| `tomas` | 50-year-old fisherman, short white beard, deep-set blue eyes, broad shoulders | green oilskin jacket and black sailor cap (never changes) |

Wardrobe changes only at a scripted time jump (night → next-morning dawn). Never mid-sequence.

## Voice bible

There are no custom voices, so each voice is a fixed phrase pasted **verbatim** before every line of dialogue, plus the same ambience phrase for each location.

| Character | Voice phrase |
|---|---|
| Maya | `a warm low alto female voice with a slight Scottish lilt, calm and measured` |
| Tomas | `a gruff deep male baritone voice, hoarse, with an Irish accent` (add `through crackling radio static` when heard over the radio) |

| Location | Ambience phrase |
|---|---|
| Lamp room | `SFX: steady rain on glass, low hum of the lamp` (becomes `heavy mechanical hum` once the lamp is lit) |
| Boat | `SFX: roaring waves, howling wind, creaking hull` |
| Dawn | `SFX: gentle waves, gulls` |

## Locations (stills)

`lamp-room` (night), `boat-deck` (night storm), `dock-dawn`, `lighthouse-dawn` (balcony).

## Film grammar used

- Each sequence opens on a wide establishing shot, moves to medium, then close-up for dialogue.
- Continuous action uses `continuesFrom`; the first shot says "Continuing directly from the previous shot" and repeats the outfit and location.
- Cross-cuts (lamp room ↔ boat) are hard cuts with a location still as `keyframes.first`.
- Ambience carries across cuts so the soundscape stays continuous; the lamp's hum is the audio motif tying the two places together.
