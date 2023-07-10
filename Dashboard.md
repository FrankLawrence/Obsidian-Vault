---
alias: Home
cssclass: dashboard
banner: "![[E3wGdziUYAENJU_.jpeg]]"
banner_x: 0.5
banner_y: 0.4
---

# Personal
- 🎥 Movies to Watch
- Friends and Family
	- [[;).canvas|;)]]
	- 

# Interests
- Security and Privacy
	- [[Darknet OpSec Bible]]
	- [[Awesome privacy]]
	- [[Steps to become more secure online]]
- Reading
	- [[A Damsel in the Desert]]
	- [[A G for James]]
	- [[Hi... Neighbor]]
	- [[Pandamic Prestige]]
	- [[Borrowing a Bite]]
- Programming
	- [[Terminal]]
	- [[Vim]]
	- [[MathJax Basic Tutorial]]
	- [[Bash Oneliners]]
	- [[Unix Command Reference.canvas|Unix Command Reference]]
# Work
- [[Uni Courses🎓]]
	- [[Linear Algebra]]
	- [[Computer Networks]]

# Recents
```dataview
TABLE file.mtime as Edited, file.folder as "Folder"
FROM "" and !"Daily Notes"
WHERE date(now) - file.mtime <= dur(3 days) and file.name != "Home🏠.md" 
SORT file.mtime desc
```

# Vault Info
- 🗄️ Recent file updates
 `$=dv.list(dv.pages('').sort(f=>f.file.mtime.ts,"desc").limit(4).file.link)`
- 🔖 Tagged:  favorite 
 `$=dv.list(dv.pages('#favorite').sort(f=>f.file.name,"desc").limit(4).file.link)`
- 〽️ Stats
	-  File Count: `$=dv.pages().length`
	-  Personal recipes: `$=dv.pages('"Family/Recipes"').length`
