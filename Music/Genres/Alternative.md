---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Alternative"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Alternative]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Alternative"
```