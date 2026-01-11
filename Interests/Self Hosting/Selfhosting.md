---
Tags: 
Created: 2025-05-22 22:39:56
---
Due to the every growing reliance on digital services run by tech giants, many are either sceptical of their data retention mechanisms, privacy policies or denial to access of their own data (sometimes as a result of permanent removal of their data/account).
The Internet, what was supposed to be a decentralized architecture, is now more centralized than ever, which especially raises alarms for those who lived under an oppressive regime.
For these reasons, many have turned to running services on their own devices, which prioritize the users interests over profits, and give them full control of their data.
# The Basics
The easiest method to hosting services, that most people come to use, is via usage of [[Docker]] containers. It provides both simple setups, but can expand to more complex systems, and most services are also configured to be run primarily through docker.
# My Homelab
This page will detail how my current homelab setup looks like, and what plans I have in the future to improve it.
The main Operating System that powers my homelab is [[Proxmox]]. It runs on a [Beelink MINI S12](https://www.bee-link.com/products/beelink-mini-s12-pro-n100) at http://192.168.178.183:8006. Currently there is no need for a Proxmox cluster since I only have one (beelink) node. On Proxmox you have the option of running two types of virtualization technologies: [[QEMU/KVM Virtual Machines]] and [[Linux Containers]]. Each have their own benefits and weaknesses, but for the most part, I run most services on a [[NixOS]] [[Virtual Machine]] either via modules in nixpkgs, or for those that do not have a module yet, via docker containers translated into [[systemd]] services via nix-docker-compose. Prior to using NixOS, I used [[Linux Containers]] from scripts on [Proxmox VE Helper-Scripts](https://community-scripts.github.io/ProxmoxVE/) (a helpful list of community created scripts which automate creation of containers). In my early stages of selfhosting, I copied and ran the scripts blindly, which *I do not recommend*.

## Hardware
### Future Investments

## Software
### What I am running
- Nginx Proxy Manager (LXC)
- Paperless-ngx (LXC)
- Adguard (LXC)
- Glance (LXC)
- Linkwarden (LXC)
- Vikunja (LXC)
- HomeAssistant (VM)
- NixOS (VM)
### Website
- show current playing song
- use anubis for blocking bots
- connect with activitypub
- [POSSE](https://indieweb.org/POSSE)
- publish obsidian vault
- publish bookmark list
- monitor user visits
- links to selfhosted services, and other online sites
- should work on any browser; super lightweight
- append .md and .txt to get pure text version of site
- Use common [[DDoS Attack#Countermeasures|countermeasures]] to DDoS attacks
## Schemas and Diagrams
Here I would like to document my own setup using a diagram, as well as other noticeable diagrams I have come across that I will take inspiration from for my future setups. 
![[Home Network.canvas|Home Network]]
# Forums and Discussions
- [[What are YOU self-hosting?]]: A Lemmy post with some of the most selfhosted applications
- [[The complete guide to building your personal self hosted server for streaming and ad-blocking]]: A Lemmy post describing a homelab setup using docker
# Tutorials and HowTos
# Resources
Throughout this page, I have links to many resources. These are what I used when starting out, and continue to use now. They mainly consists of the following:
## Youtube Channels
- [Jeff Geerling](https://www.youtube.com/channel/UCR-DXc1voovS8nhAvccRZhg)
- [Hardware Haven](https://www.youtube.com/channel/UCgdTVe88YVSrOZ9qKumhULQ)
- [Raid Owl](https://www.youtube.com/channel/UC9evhW4JB_UdXSLeZGy8lGw)
- [Christian Lempa](https://www.youtube.com/channel/UCZNhwA1B5YqiY1nLzmM0ZRg)
- [Jim's Garage](https://www.youtube.com/channel/UCUUTdohVElFLSP4NBnlPEwA)
- [Wolfgang's Channel](https://www.youtube.com/channel/UCsnGwSIHyoYN0kiINAGUKxg)
- [Tailscale](https://www.youtube.com/channel/UCcdv38QxPjSMqbt5ffLhJLA)
- [Lawrence Systems](https://www.youtube.com/channel/UCHkYOD-3fZbuGhwsADBd9ZQ)

## Forums
- [r\homelab](https://www.reddit.com/r/homelab/)
- [r\selfhosted](https://www.reddit.com/r/selfhosted/)

## Awesome Lists
- [[Interests/Self Hosting/awesome-selfhosted/README|Awesome Selfhosted]]: https://github.com/awesome-selfhosted/awesome-selfhosted
- Awesome

## Blogs
Some of these blogs are from the above mentioned Youtube channels
- https://thehomelabber.com/guides/
- https://b3n.org/
- https://beingmanan.com/wp/
- https://www.nijho.lt/
- https://blog.fidelramos.net/software/homelab
- https://mtlynch.io/building-a-vm-homelab/
## Homelab Repositories and Configurations
## Hardware
- [Sparsamsten Systeme](https://docs.google.com/spreadsheets/d/1LHvT2fRp7I6Hf18LcSzsNnjp10VI-odvwZpQZKv_NCI/edit?gid=0#gid=0)

---
References: