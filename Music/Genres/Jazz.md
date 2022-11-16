---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Jazz"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Jazz]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Jazz"
```