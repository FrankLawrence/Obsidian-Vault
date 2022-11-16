---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Progressive Rock"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Progressive Rock]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Progressive Rock"
```