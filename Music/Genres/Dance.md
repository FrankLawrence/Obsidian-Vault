---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Dance"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Dance]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Dance"
```