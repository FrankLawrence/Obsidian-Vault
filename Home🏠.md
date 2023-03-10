---
tags: [MOC]
alias: ["000", "Start"]
---
This is your launchpad and homebase. Where I can orientate myself. Your personal North Star. That's here. That's home.

### Main Maps
- 01 - 1- [[School📚🏫]]
- 01 - 2 - [[Uni Courses🎓]]
- 01 - 3 - [[MIT Opencourseware]]
- 02 - [[Personal|Personal MOC]]
- 03 - [[Interests|Interests MOC]]
- 04 - [[Goals|Goals MOC]]
- 05 - [[Other]]
# Recents
```dataview
TABLE file.mtime as Edited, file.folder as "Folder"
FROM "" and !"Daily Notes"
WHERE date(now) - file.mtime <= dur(3 days) and file.name != "Home🏠.md" 
SORT file.mtime desc
```

```dataview
TABLE file.ctime as Created, file.folder as "Folder"
FROM ""
WHERE date(now) - file.ctime <= dur(3 days)
SORT file.ctime desc
```
