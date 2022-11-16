---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "New Wave"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[New Wave]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "New Wave"
```