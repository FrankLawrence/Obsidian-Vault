---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Hip-Hop"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Hip-Hop]]
```
Albums
---
```dataview
LIST FROM "Music/Albums" 
WHERE Genre = "Hip-Hop"
```