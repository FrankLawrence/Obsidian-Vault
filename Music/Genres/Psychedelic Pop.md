---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Psychedelic Pop"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Psychedelic Pop]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Psychedelic Pop"
```