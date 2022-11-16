---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Rock"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Rock]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Rock"
```