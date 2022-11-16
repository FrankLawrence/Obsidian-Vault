---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Pop Rock"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Pop Rock]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Pop Rock"
```