---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Art rock"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Art rock]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Art rock"
```