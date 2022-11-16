---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Soft Rock"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Soft Rock]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Soft Rock"
```