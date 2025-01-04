> Some tools at [[Tools & Plugins#Blender]]

When using Rigify, to make the mirror X feature work bones must be named the same with the `.r`/`.l` suffix (it doesn't care about capitalization). Using anything else than `.` as the separator will make it break (maybe there is a setting to change that somewhere).
Luckily, the "Send 2 UE" addon will replace all `.` with `_` in UE.
