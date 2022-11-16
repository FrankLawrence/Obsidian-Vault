---
tags: Artist
Created: <%tp.date.tomorrow()%>
---
[Genre:: ]

Songs
---
```dataview
TABLE Year, Album, tags AS "Tags" FROM [[<%tp.file.title%>]] AND "Music/Songs"
SORT file.tags desc
```
Albums
---
```dataview
TABLE Year, Genre FROM [[<%tp.file.title%>]] AND "Music/Albums"
SORT Year
```