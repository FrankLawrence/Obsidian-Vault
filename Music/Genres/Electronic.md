---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Electronic"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Electronic]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Electronic"
```