---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Blues"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Blues]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Blues"
```