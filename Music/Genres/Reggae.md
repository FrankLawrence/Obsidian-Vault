---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Reggae"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Reggae]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Reggae"
```