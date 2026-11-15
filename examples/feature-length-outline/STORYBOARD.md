# Continuity ledger

STYLE: ligne claire cartoon style, clean bold outlines, flat cel-shaded colors

| Scene | Seq | Location still | Character (variant) | Time | Entry state | Exit state | Link |
|---|---|---|---|---|---|---|---|
| s001-dawn | 1 | lighthouse-dawn | maya | dawn | empty balcony | Maya at rail, cup in hand, facing sea (screen right) | keyframe `lighthouse-dawn` |
| s002-balcony | 1 | lighthouse-dawn | maya | dawn | as s001 exit | Maya raising binoculars | `continuesFrom: s001-dawn` |
| s003-lamp-room | 2 | lighthouse-night | maya-night | night | Maya at radio | Maya listening, hand on dial | hard cut, keyframe `lighthouse-night` |
| s004-signal | 2 | lighthouse-night | maya-night | night | as s003 exit | lamp beam sweeping | `continuesFrom: s003-lamp-room` |
| s005-dawn-after | 3 | lighthouse-dawn | maya | dawn (next day) | calm sea | Maya smiling | hard cut, keyframe `lighthouse-dawn` |
