---
Tags: Linux
Created: 2025-12-03 22:47:18
---
> [!question] What is the difference between Xorg, Wayland, X11, Hyprland ... ?
> When setting up a system, we come across terms like *compositors*, *tiling window managers*, desktop environments, etc.
> This explains the basics; enough to have an idea of what to setup.

- X11 (X version 11) is a **protocol** that the kernel uses to communicate with the **display server** (just like an API), in this case with the X.Org server
	- A display server is tasked with drawing and moving windows on the display device and working with input like mouse and keyboard
	- X protocol is version 11 since 1987 is *outdated*
- 

---
References:
- https://unix.stackexchange.com/questions/596894/how-does-linuxs-display-work