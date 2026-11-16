# Continuity ledger

| Scene | Location still | Characters | Time | Entry state | Exit state | Link |
|---|---|---|---|---|---|---|
| s001-sherwood-dawn | sherwood-bridge | robin | dawn | empty bridge | Robin steps onto the bridge | keyframe `sherwood-bridge` |
| s002-bridge-meeting | sherwood-bridge | robin, little-john | dawn | as s001 exit | Robin sets down his bow | `continuesFrom: s001-sherwood-dawn` |
| s003-staff-fight | sherwood-bridge | robin, little-john | dawn | as s002 exit | John in the stream, laughing | `continuesFrom: s002-bridge-meeting` |
| s004-tournament | archery-field | sheriff, robin | afternoon | crowd, pavilion | Sheriff sneering | hard cut, keyframe `archery-field` |
| s005-the-shot | archery-field | robin, sheriff | afternoon | as s004 exit | arrow splits the bullseye | `continuesFrom: s004-tournament` |
| s006-escape | archery-field | robin, little-john, sheriff | afternoon | as s005 exit | heroes run into the trees | `continuesFrom: s005-the-shot` |
