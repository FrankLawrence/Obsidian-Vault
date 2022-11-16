---
tags: Artist
Created: 2022-10-21 
---
[Genre:: ]

Songs
---
```dataview
TABLE Year, Album, tags AS "Tags" FROM [[ACDC]] AND "Music/Songs"
SORT file.tags desc
```
Albums
---
```dataview
TABLE Year, Genre FROM [[ACDC]] AND "Music/Albums"
SORT Year
```