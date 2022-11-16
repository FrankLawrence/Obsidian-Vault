---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Anime"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Anime]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Anime"
```