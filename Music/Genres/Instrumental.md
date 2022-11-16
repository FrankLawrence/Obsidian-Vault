---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Instrumental"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Instrumental]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Instrumental"
```