---
Tags:
---
(Links:: [[2021-W<% tp.date.now("WW", 7) %>]])

<< [[<% tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") %>]] - [[<% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %>]] >>
## Favorite song
(Favorite_Song:: )

___
## Today's Notes
```dataview
LIST FROM !"Daily Notes"
WHERE file.cday = date(<%tp.file.title%>)
```
## Modified Notes
```dataview
TABLE file.mtime AS "Modified", file.cday AS "Created" FROM !"Daily Notes" 
WHERE file.mtime > date(<%tp.file.title%>) AND file.mtime < date(<% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %>) AND file.cday < date(<%tp.file.title%>)
SORT file.mtime desc
```
___
