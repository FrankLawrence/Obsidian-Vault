---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Classical"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Classical]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Classical"
```