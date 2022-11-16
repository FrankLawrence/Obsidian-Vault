---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Soul"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Soul]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Soul"
```