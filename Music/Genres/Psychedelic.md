---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Psychedelic"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Psychedelic]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Psychedelic"
```