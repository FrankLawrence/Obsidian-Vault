---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Alternative Rock"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Alternative Rock]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Alternative Rock"
```