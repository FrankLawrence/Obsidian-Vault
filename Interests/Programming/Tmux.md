---
Tags: 
Created: 2022-10-14 04:34:28
---
(Links:: [[Programming]])

| Command                   | Explanation                                    |
| ------------------------- | ---------------------------------------------- |
| tmux                      | Start new session                              |
| tmux new -s name          | Start new session with name                    |
| tmux ls                   | List all the sessions                          |
| tmux kill-session -t name | Kill the mentioned session                     |
| tmux kill-session -a      | Kill all sessions except the last used session |
| tmux kill-server          | Kill all tmux sessions                         |
| tmux attach -t name       | Attach/enter the mentioned session             |
| [Ctrl +b] + d             | Detach/exit the current session                |
| [Ctrl +b] + $             | Name or rename current session                 |
| [Ctrl +b] + c             | Create new window                              |
| [Ctrl +b] + ,             | Name or rename current window                  |
| [Ctrl +b] + w             | List all windows                               |
| [Ctrl +b] + &             | Kill the current window                        |
| [Ctrl +b] + n/p/N         | Move to next/previous/Nth window               |
| [Ctrl +b] + %             | Create horizontal pane                         |
| [Ctrl +b] + “             | Create vertical pane                           |
| [Ctrl +b] + space key     | Toggle the layout of current pane              |
| [Ctrl +b] + z             | Zoom into the current pane                     |
| [Ctrl +b] + x             | Kill the current pane                          |
| [Ctrl +b] + ?             | Show list of commands                          |
| [Ctrl +b] + s             | list sessions                                               |
___
References: