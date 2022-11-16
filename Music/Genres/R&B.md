---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "R&B"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[R&B]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "R&B"
```