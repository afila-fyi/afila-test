# Continuity ledger

| Scene | Seq | Location still | Character (variant) | Time | Entry state | Exit state | Link |
|---|---|---|---|---|---|---|---|
| s001-lamp-room | 1 | lamp-room | maya-night | night storm | empty room, lamp dim | Maya at desk, looking up at radio | keyframe `lamp-room` |
| s002-radio | 1 | lamp-room | maya-night | night | as s001 exit | leaning to radio, hand on dial | `continuesFrom: s001-lamp-room` |
| s003-lever | 1 | lamp-room | maya-night | night | as s002 exit | lamp beam turning | `continuesFrom: s002-radio` |
| s004-boat-deck | 2 | boat-deck | tomas | night storm | wave hits | Tomas fighting the wheel | cross-cut, keyframe `boat-deck` |
| s005-mayday | 2 | boat-deck | tomas | night | as s004 exit | holding the radio | `continuesFrom: s004-boat-deck` |
| s006-beam | 2 | boat-deck | tomas | night | as s005 exit | beam lights his face, relief | `continuesFrom: s005-mayday` |
| s007-maya-answers | 2 | lamp-room | maya-night | night | lamp blazing | speaking into radio | cross-cut, keyframe `lamp-room` |
| s008-beam-sweep | 2 | lamp-room | maya-night | night | as s007 exit | at window, smiling | `continuesFrom: s007-maya-answers` |
| s009-dock | 3 | dock-dawn | tomas | next-day dawn | calm sea | Tomas looks up at lighthouse | time jump, keyframe `dock-dawn` |
| s010-balcony | 3 | lighthouse-dawn | maya (raincoat) | dawn | empty balcony | raising mug toward dock | hard cut, keyframe `lighthouse-dawn` |
| s011-meeting | 3 | dock-dawn | maya, tomas | dawn | meeting on the steps | two-shot, smiling | hard cut, keyframe `dock-dawn` |
| s012-thanks | 3 | dock-dawn | maya, tomas | dawn | as s011 exit | Tomas grinning | `continuesFrom: s011-meeting` |
| s013-ascent | 3 | dock-dawn → lighthouse | maya, tomas | dawn | as s012 exit | wide of sunlit lighthouse | `continuesFrom: s012-thanks` |
