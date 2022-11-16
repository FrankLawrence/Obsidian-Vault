---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Disco"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Disco]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Disco"
```