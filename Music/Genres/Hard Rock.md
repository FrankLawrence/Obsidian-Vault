---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Hard Rock"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Hard Rock]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Hard Rock"
```