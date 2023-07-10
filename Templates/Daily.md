---
Tags:
---
(Links:: [[2021-W<% tp.date.now("WW", 7) %>]])

<< [[<%tp.date.yesterday()%>]] - [[<%tp.date.tomorrow()%>]] >>
## Reminders
- ![[My Daily Laws]]
- ![[<%tp.date.yesterday()%>#Tomorrows tasks]]
### Birthdays
```dataview
LIST FROM #people 
WHERE date(Birthday).month = date({{date:YYYY-MM-DD}}).month AND date(Birthday).day = date({{date:YYYY-MM-DD}}).day

```
### Today's Tasks
```todoist 
name: Highest Priority & Date 
filter: "today | overdue" 
sorting: 
- date 
- priority
group: true 
```
> Refer to ![[To Do's (Tq)]]
---
- [ ] <%tp.file.cursor(1)%>
<%* if (tp.date.now("ddd") == "Sun") { %>
- [ ] Make Weekly Note 
<%* } %>
<%* if (tp.date.now("D") == 1) { %>
- [ ] Make Monthly Note
<%* } %>
<%* if (tp.date.now("M-D") == "1-1") { %>
- [ ] Make Yearly Note
<%* } %>
### Tomorrows tasks
- [ ] %%<%tp.file.cursor(2)%>%%
### Week tasks
### Schedule
<%tp.file.include("[[Dailies]]")%>
## Links
```dataview
LIST FROM [[<%tp.date.now()%>]] AND !"Daily Notes"
```
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
