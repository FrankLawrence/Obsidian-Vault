---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Electro-pop"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Electro-pop]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Electro-pop"
```