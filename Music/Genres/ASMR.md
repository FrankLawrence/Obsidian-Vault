---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "ASMR"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[ASMR]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "ASMR"
```