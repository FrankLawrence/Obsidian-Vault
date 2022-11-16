---
Created: <%tp.date.tomorrow()%>
---
Songs
---
```dataview
TABLE Artist, Year FROM "Music/Songs"
WHERE Genre = "<%tp.file.title%>"
```
Artists
---
```dataview
LIST FROM "Music/Artists" AND [[<%tp.file.title%>]]
```
Albums
---
```dataview
LIST FROM "Music/Albums"
WHERE Genre = "<%tp.file.title%>"
```