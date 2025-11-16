---
Tags:
Created: 2025-04-02 01:37:43
---
I run everything in docker on Ubuntu 22.04 with the exception of Plex, which runs on bare metal on the same server. The server is a 16 core threadripper 1950, with 2 quadro gpu’s, m2000 and a p400, 128gb ram, mirrored ssd for system, platter HDD for media, CoralTPU pcie.

I also run Home Assistant on a separate Lenovo MiniPC(forget which model), I did this so I can take down the server for various reasons without losing smart home stuff. Helps with the Partner Acceptance Factor.

In no particular order the server runs:

Calibre-web - Library management

Sonarr - TV series downloads

Radarr - Movie Downloads

Lidarr - Music Downloads

QbittorentVPN - Torrents over vpn, guarantees no leaks

Jackett - tracker management and proxying

Podgrab - downloads podcasts

Frigate - NVR, camera recording with object detection

DoubleTake - Facial recognition middleware, works between frigate/homeassistant and Compreface/Deepstack

Octoprint - 3d printer spooler

Tautulli - Plex statistics

Portainer - Docker Management

Ombi - Media request app, users can request shows/movies and they can be automatically added to sonarr/radarr

MeTube - Webui for youtube-dl/dlp, useful for downloading Youtube videos for offline and ad free use

Spot-dl - parses spotify playlists and downloads them from youtube

> ## Fedi servers
> 
> - lemmy.world
> - mastodon.world
> - calckey.world
> - pool.social
> - musicworld.social
> - [akkoma.nl](http://akkoma.nl)
> - ruud.social
> - [fotofed.nl](http://fotofed.nl)
> - [fediland.nl](http://fediland.nl)
> - blog.mastodon.world
> - play-my.video
> 
> ## Software I use
> 
> - Nginx Proxy Manager
> - Portainer
> - Kimai
> - Xwiki (3 of them)
> - Cryptpad
> - Grafana
> - Hedgedoc
> - Matrix/Synapse
> - Thelounge
> - Vaultwarden
> - Gitea
> - Nextcloud
> - Paperless-ngx
> - Zabbix
> - Zammad
> 
> ~ Ruud

> My long and mostly complete list:
> 
> - [Audiobookshelf](https://www.audiobookshelf.org/) ([GH](https://github.com/advplyr/audiobookshelf))
>     - Using for audiobooks. Ebooks, comics, and podcast support in early stages.
> - [Authelia](https://www.authelia.com/) ([GH](https://github.com/authelia/authelia))
>     - Using for two-factor authentication in front of all of my services. Critical infrastructure.
> - [Bazarr](https://www.bazarr.media/) ([GH](https://github.com/linuxserver/docker-bazarr))
>     - Using for automated subtitle management. Have not needed to rely on it much.
> - [Code-Server](https://coder.com/) ([GH](https://github.com/linuxserver/docker-code-server))
>     - Using for a plethora of things. I could write an entire post on this alone.
> - [Courier](https://github.com/tborychowski/courier)
>     - Using (occasionally) for package-tracking from various carriers.
> - [EmulatorJS](https://github.com/linuxserver/docker-emulatorjs)
>     - Using for retro-emulation.
> - [Gitea](https://gitea.com/) ([GH](https://github.com/go-gitea/gitea)) x2
>     - Using as a git repo server, package repository, and for CI/CD automation. Is critical infrastructure in my lab. Could also write an entire post on this one.
> - [Headscale](https://github.com/juanfont/headscale) with [Headscale-UI](https://github.com/gurucomputing/headscale-ui). Tailscale clients on various VMs LXCs, etc.
>     - Using to securely network with my remote servers.
> - [Homepage](https://github.com/benphelps/homepage)
>     - Using as a “single-pane-of-glass” to get an overview of service health with links to the various services.
> - [Invidious](https://github.com/iv-org/invidious)
>     - Using in-place of YouTube.
> - [IT-Tools](https://it-tools.tech/) ([GH](https://github.com/CorentinTh/it-tools))
>     - Using for the myriad of various useful tools it offers.
> - [Jellyfin](https://jellyfin.org/) ([GH](https://github.com/linuxserver/docker-jellyfin))
>     - My media player of choice. Using for movies and television, but supports music, ebooks, and photos in addition.
> - [Kopia Server](https://kopia.io/) ([GH](https://github.com/kopia/kopia))
>     - Using for data backups to my Minio instance on local NAS and Wasabi. Simple, fast, and reliable.
> - [Librespeed](https://librespeed.org/) ([GH](https://github.com/linuxserver/docker-librespeed))
>     - Using for the occasional speedtest to my remote servers.
> - Matrix stack using [Conduit](https://gitlab.com/famedly/conduit/) back end and [Element-Web](https://gitlab.com/famedly/conduit/) front end
>     - Federated Discord essentially. Using as a private instance for friends and family.
> - [Minio](https://github.com/minio/minio)
>     - Using primarily as a gateway to storing backups, also serves git-lfs for Gitea.
> - [N8N](https://n8n.io/) ([GH](https://github.com/n8n-io/n8n))
>     - Using for home-automation, backing up my Reddit saved posts to a database, deal-alerts, and part of a CI/CD pipeline.
> - [NTFY](https://ntfy.sh/) ([GH](https://github.com/binwiederhier/ntfy))
>     - Using for infrastructure notifications mostly. Very simple and versatile alerting solution.
> - [NZBGet](https://github.com/linuxserver/docker-nzbget)
>     - Using for getting “usenet articles”.
> - [Paperless-NGX](https://github.com/paperless-ngx/paperless-ngx)
>     - Using for document archival. Important receipts, documentation, letters, etc. live here.
> - [Portainer](https://www.portainer.io/) ([GH](https://github.com/portainer/portainer)) with multiple agents on VM’s LXCs and VPSs
>     - High level management of my various docker containers.
> - [Prowlarr](https://github.com/linuxserver/docker-prowlarr)
>     - Using to provide torznab API to websites that dont natively have it. Integrates with Radarr and Sonarr
> - [Radarr](https://radarr.video/) ([GH](https://github.com/linuxserver/docker-radarr))
>     - Using for movie management.
> - [Radicale](https://github.com/tomsquest/docker-radicale)
>     - Using for contacts and calendar server.
> - [Raneto](https://raneto.com/) ([GH](https://github.com/linuxserver/docker-raneto))
>     - Using as a knowledge base. Lab documentation, lists, recipes, lots of things live here. Using with with code-server and Gitea.
> - [Readarr](http://readarr.com/) ([GH](https://github.com/linuxserver/docker-readarr))
>     - Using for book management
> - [Recyclarr](https://recyclarr.dev/) ([GH](https://github.com/recyclarr/recyclarr))
>     - Using for Radar and Sonarr to sync search terms for their automations. Very useful, hard to summarize.
> - [Requestrr](https://github.com/linuxserver/docker-requestrr)
>     - Using (very rarely) as a requests bot for Radarr and Sonarr.
> - [SFTP-Go](https://github.com/drakkan/sftpgo)
>     - Using mostly in-place of Nextcloud. Used to back up phones mostly.
> - [Shaarli](https://shaarli.readthedocs.io/en/master/) [(GH](https://github.com/shaarli/Shaarli))
>     - Using as a read-it-later service. Went through lots of these, and Shaarli has been good enough.
> - [Singlefile-Archive](https://github.com/simonw/datasette)
>     - A hacky way of presenting pages saved with the singlefile browser extension. Not exactly happy with the solution, but for my ocasional use it does work.
> - [Sonarr](https://sonarr.tv/) ([GH](https://github.com/linuxserver/docker-sonarr))
>     - Using as TV series manager
> - [Speedtest-Tracker](https://docs.speedtest-tracker.dev/) ([GH](https://github.com/alexjustesen/speedtest-tracker))
>     - Using to get periodic speedtests. Plan to automate results to blast my ISP if my service speed gets too low.
> - [Traefik](https://traefik.io/) ([GH](https://github.com/traefik/traefik)) on each seperate host
>     - Using as a web proxy in front of my various services. Critical infrastructure.
> - [Transmission](https://transmissionbt.com/) ([GH](https://github.com/linuxserver/docker-transmission))
>     - Using to get “Linux ISOs”
> - [Uptime Kuma](https://uptime.kuma.pet/) ([GH](https://github.com/louislam/uptime-kuma))
>     - Using to monitor site and services status along with a few others. Integrated with NTFY for alerts.
> - [Vaultwarden](https://github.com/dani-garcia/vaultwarden)
>     - Using as my password manager. Have been using for years, cannot recommend enough.
> - A handful of static websites served with [NGINX](https://hub.docker.com/_/nginx)
>     - The old standby, its been reliable as a webserver.
> 
> These services are the result of years of development and administrating my lab and while there is still some cruft, it’s mostly services that I think have real utility.
> 
> As far as hardware:
> 
> - Running pfsense on a toughbook laptop as a router-firewall.
>     
> - A SuperMicro 24 bay disk-shelf with Proxmox and ZFS for NAS duties and a couple services.
>     
> - Lenovo Tiny boxes with a Proxmox cluster for the majority of my local services.
>     
> - Dell managed switch
>     
> - A few Raspberry-pi’s with Raspbian for various things.
>     
> - Linksys AP for wifi
> 
> ~ sneakyninjapants

___
References:
- https://lemmy.world/post/75568