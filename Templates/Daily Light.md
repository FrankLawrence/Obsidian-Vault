---
Tags:
---
(Links:: [[2021-W<% tp.date.now("WW", 7) %>]])

<< [[<%tp.date.yesterday()%>]] - [[<%tp.date.tomorrow()%>]] >>
## Favorite song
(Favorite_Song:: )
___
## Today's Notes
```dataview
LIST FROM !"Daily Notes"
WHERE file.cday = date(<%tp.date.now()%>)
```
## Modified Notes
```dataview
TABLE file.mtime AS "Modified", file.cday AS "Created" FROM !"Daily Notes" 
WHERE file.mtime > date(<%tp.date.now()%>) AND file.mtime < date(<%tp.date.tomorrow()%>) AND file.cday < date(<%tp.date.now()%>)
SORT file.mtime desc
```
___
