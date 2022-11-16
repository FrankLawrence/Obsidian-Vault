---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "J-Pop"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[J-Pop]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "J-Pop"
```