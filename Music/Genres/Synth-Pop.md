---
Created: 2022-10-21 
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "Synth-Pop"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[Synth-Pop]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "Synth-Pop"
```