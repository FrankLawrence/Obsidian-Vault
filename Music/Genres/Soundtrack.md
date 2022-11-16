---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Soundtrack"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Soundtrack]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Soundtrack"
```