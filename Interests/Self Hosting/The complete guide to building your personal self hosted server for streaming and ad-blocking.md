---
Tags:
Created: 2025-04-02 01:27:44
---

*Captain’s note: This `OC` [was originally posted in reddit](https://www.reddit.com/r/Piracy/comments/pqsomd/the_complete_guide_to_building_your_personal_self/) but it’s quality makes me wants to ensure a copy survices in lemmy as well.*

---

We will setup the following applications in this guide:

- **Docker**
- **AdguardHome** - Adblocker for all your devices
- **Jellyfin/Plex** - For watching the content you download
- **Qbittorrent** - Torrent downloader
- **Jackett** - Torrent indexers provider
- **Flaresolverr** - For auto solving captcha in some of the indexers
- **Sonarr** - \*arr service for automatically downloading TV shows
- **Radarr** - \*arr service for movies
- **Readarr** - \*arr service for (audio)books
- **lidarr** - \*arr service for music
- **Bazarr** - Automatically downloads subtitles for Sonarr and Radarr
- **Ombi/Overseer** - For requesting movies and tv shows through Sonarr and Radarr
- **Heimdall** - Dashboard for all the services so you don’t need to remember all the ports

Once you are done, your dashboard will look something like this.

​

[Heimdall Dashboard](https://lemmy.dbzer0.com/pictrs/image/779256bf-4eae-48fa-a942-c960a3f556dc.png)

I started building my setup after reading this guide [https://www.reddit.com/r/Piracy/comments/ma1hlm/the\_complete\_guide\_to\_building\_your\_own\_personal/](https://www.reddit.com/r/Piracy/comments/ma1hlm/the_complete_guide_to_building_your_own_personal/).

## Hardware

You don’t need powerful hardware to set this up. I use a decade old computer, with the following hardware. Raspberry pi works fine.

​

[Hardware](https://lemmy.dbzer0.com/pictrs/image/07685cac-36d7-4270-b3a7-c156e232829c.png)

## Operating system

I will be using **Ubuntu** **server** in this guide. You can select whatever linux distro you prefer.

Download ubuntu server from [https://ubuntu.com/download/server](https://ubuntu.com/download/server). Create a bootable USB drive using [rufus](https://rufus.ie/en/) or any other software(I prefer [ventoy](https://www.ventoy.net/en/index.html)). Plug the usb on your computer, and select the usb drive from the boot menu and install ubuntu server. Follow the steps to install and configure ubuntu, and make sure to check “**Install OpenSSH server**”. Don’t install docker during the setup as the snap version is installed.

Once installation finishes you can now reboot and connect to your machine remotely using ssh.

```
ssh username@server-ip 
# username you selected during installation
# Type ip a to find out the ip address of your server. Will be present against device like **enp4s0** prefixed with 192.168.
```

## Create the directories for audiobooks, books, movies, music and tv.

I keep all my media at ~/server/media. If you will be using multiple drives you can look up how to mount drives.

We will be using [hardlinks](https://trash-guides.info/Hardlinks/Hardlinks-and-Instant-Moves/) so once the torrents are downloaded they are linked to media directory as well as torrents directory without using double storage space. Read up the [trash-guides](https://trash-guides.info/Hardlinks/Hardlinks-and-Instant-Moves/) to have a better understanding.

```
mkdir ~/server
mkdir ~/server/media # Media directory
mkdir ~/server/torrents # Torrents

# Creating the directories for torrents
cd ~/server/torrents
mkdir audiobooks  books  incomplete  movies  music  tv 

cd ~/server/media
mkdir audiobooks  books  movies  music  tv
```

## Installing docker and docker-compose

**Docker** [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

```
# install packages to allow apt to use a repository over HTTPS
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
# Add Docker’s official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
# Setup the repository
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# Install Docker Engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
# Add user to the docker group to run docker commands without requiring root
sudo usermod -aG docker $(whoami) 
```

> Sign out by typing exit in the console and then ssh back in

**Docker compose** [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

```
# Download the current stable release of Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# Apply executable permissions to the binary
sudo chmod +x /usr/local/bin/docker-compose
```

## Creating the compose file for Adguard home

First setup Adguard home in a new compose file.

Docker compose uses a yml file. All of the files contain version and services object.

Create a directory for keeping the compose files.

```
mkdir ~/server/compose
mkdir ~/server/compose/adguard-home
vi ~/server/compose/adguard-home/docker-compose.yml
```

Save the following content to the docker-compose.yml file. You can see [here](https://hub.docker.com/r/adguard/adguardhome) what each port does.

```
version: '3.3'
services:
    run:
        container_name: adguardhome
        restart: unless-stopped
        volumes:
            - '/home/${USER}/server/configs/adguardhome/workdir:/opt/adguardhome/work'
            - '/home/${USER}/server/configs/adguardhome/confdir:/opt/adguardhome/conf'
        ports:
            - '53:53/tcp'
            - '53:53/udp'
            - '67:67/udp'
            - '68:68/udp'
            - '68:68/tcp'
            - '80:80/tcp'
            - '443:443/tcp'
            - '443:443/udp'
            - '3000:3000/tcp'
        image: adguard/adguardhome
```

Save the file and start the container using the following command.

```
docker-compose up -d
```

Open up the Adguard home setup on `YOUR_SERVER_IP:3000`.

Enable the default filter list from filters→DNS blocklist. You can then add custom filters.

[Filters](https://lemmy.dbzer0.com/pictrs/image/57e11e55-08b0-4b11-98e6-2f3b8d14be86.png)

## Creating the compose file for media-server

## Jackett

Jackett is where you define all your torrent indexers. All the \*arr apps use the tornzab feed provided by jackett to search torrents.

There is now an \*arr app called prowlarr that is meant to be the replacement for jackett. But the flaresolverr(used for auto solving captchas) support was added very recently and doesn’t work that well as compared to jackett, so I am still sticking with jackett for meantime. You can instead use prowlarr if none of your indexers use captcha.

```
jackett:
    container_name: jackett
    image: linuxserver/jackett
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - '/home/${USER}/server/configs/jackett:/config'
      - '/home/${USER}/server/torrents:/downloads'
    ports:
      - '9117:9117'
    restart: unless-stopped
prowlarr:
		container_name: prowlarr
    image: 'hotio/prowlarr:testing'
    ports:
      - '9696:9696'
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - '/home/${USER}/server/configs/prowlarr:/config'
    restart: unless-stopped
```

## Sonarr - TV

Sonarr is a TV show scheduling and searching download program. It will take a list of shows you enjoy, search via Jackett, and add them to the qbittorrent downloads queue.

```
sonarr:
    container_name: sonarr
    image: linuxserver/sonarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    ports:
      - '8989:8989'
    volumes:
      - '/home/${USER}/server/configs/sonarr:/config'
      - '/home/${USER}/server:/data'
    restart: unless-stopped
```

## Radarr - Movies

Sonarr but for movies.

```
radarr:
    container_name: radarr
    image: linuxserver/radarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    ports:
      - '7878:7878'
    volumes:
      - '/home/${USER}/server/configs/radarr:/config'
      - '/home/${USER}/server:/data'
    restart: unless-stopped
```

## Lidarr - Music

```
lidarr:
    container_name: lidarr
    image: ghcr.io/linuxserver/lidarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - '/home/${USER}/server/configs/liadarr:/config'
      - '/home/${USER}/server:/data'
    ports:
      - '8686:8686'
    restart: unless-stopped
```

## Readarr - Books and AudioBooks

```
# Notice the different port for the audiobook container
readarr:
    container_name: readarr
    image: 'hotio/readarr:nightly'
    ports:
      - '8787:8787'
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - '/home/${USER}/server/configs/readarr:/config'
      - '/home/${USER}/server:/data'
    restart: unless-stopped

readarr-audio-books:
    container_name: readarr-audio-books
    image: 'hotio/readarr:nightly'
    ports:
      - '8786:8787'
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - '/home/${USER}/server/configs/readarr-audio-books:/config'
      - '/home/${USER}/server:/data'
    restart: unless-stopped
```

## Bazarr - Subtitles

```
bazarr:
    container_name: bazarr
    image: ghcr.io/linuxserver/bazarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - '/home/${USER}/server/configs/bazarr:/config'
      - '/home/${USER}/server:/data'
    ports:
      - '6767:6767'
    restart: unless-stopped
```

## Jellyfin

I personally only use jellyfin because it’s completely free. I still have plex installed because overseerr which is used to request movies and tv shows require plex. But that’s the only role plex has in my setup.

I will talk about the devices section later on.

For the media volume you only need to provide access to the `/data/media` directory instead of `/data` as jellyfin doesn’t need to know about the torrents.

```
jellyfin:
    container_name: jellyfin
    image: ghcr.io/linuxserver/jellyfin
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    ports:
      - '8096:8096'
    devices:
      - '/dev/dri/renderD128:/dev/dri/renderD128'
      - '/dev/dri/card0:/dev/dri/card0'
    volumes:
      - '/home/${USER}/server/configs/jellyfin:/config'
      - '/home/${USER}/server/media:/data/media'
    restart: unless-stopped

plex:
    container_name: plex
    image: ghcr.io/linuxserver/plex
    ports:
      - '32400:32400'
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
      - VERSION=docker
    volumes:
      - '/home/${USER}/server/configs/plex:/config'
      - '/home/${USER}/server/media:/data/media'
    devices:
      - '/dev/dri/renderD128:/dev/dri/renderD128'
      - '/dev/dri/card0:/dev/dri/card0'
    restart: unless-stopped
```

## Overseer/Ombi - Requesting Movies and TV shows

I use both. You can use ombi only if you don’t plan to install plex.

```
ombi:
    container_name: ombi
    image: ghcr.io/linuxserver/ombi
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - '/home/${USER}/server/configs/ombi:/config'
    ports:
      - '3579:3579'
    restart: unless-stopped

overseerr:
    container_name: overseerr
    image: ghcr.io/linuxserver/overseerr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - '/home/${USER}/server/configs/overseerr:/config'
    ports:
      - '5055:5055'
    restart: unless-stopped
```

## Qbittorrent - Torrent downloader

I use [qflood](https://hotio.dev/containers/qflood/) container. Flood provides a nice UI and this image automatically manages the connection between qbittorrent and flood.

Qbittorrent only needs access to torrent directory, and not the complete data directory.

```
qflood:
    container_name: qflood
    image: hotio/qflood
    ports:
      - "8080:8080"
      - "3005:3000"
    environment:
      - PUID=1000
      - PGID=1000
      - UMASK=002
      - TZ=Asia/Kolkata
      - FLOOD_AUTH=false
    volumes:
      - '/home/${USER}/server/configs/qflood:/config'
      - '/home/${USER}/server/torrents:/data/torrents'
    restart: unless-stopped
```

## Heimdall - Dashboard

There are multiple dashboard applications but I use Heimdall.

```
heimdall:
    container_name: heimdall
    image: ghcr.io/linuxserver/heimdall
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - '/home/${USER}/server/configs/heimdall:/config'
    ports:
      - 8090:80
    restart: unless-stopped
```

## Flaresolverr - Solves cloudflare captcha

If your indexers use captcha, you will need flaresolverr for them.

```
flaresolverr:
    container_name: flaresolverr
    image: 'ghcr.io/flaresolverr/flaresolverr:latest'
    ports:
      - '8191:8191'
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    restart: unless-stopped
```

## Transcoding

As I mentioned in the jellyfin section there is a section in the conmpose file as “devices”. It is used for [transcoding](https://en.wikipedia.org/wiki/Transcoding). If you don’t include that section, whenever transcoding happens it will only use CPU. In order to utilise your gpu the devices must be passed on to the container.

[https://jellyfin.org/docs/general/administration/hardware-acceleration.html](https://jellyfin.org/docs/general/administration/hardware-acceleration.html) Read up this guide to setup hardware acceleration for your gpu.

Generally, the devices are same for intel gpu transcoding.

```
devices:
      - '/dev/dri/renderD128:/dev/dri/renderD128'
      - '/dev/dri/card0:/dev/dri/card0'
```

To monitor the gpu usage install `intel-gpu-tools`

```
sudo apt install intel-gpu-tools
```

Now, create a compose file for media server.

```
mkdir ~/server/compose/media-server
vi ~/server/compose/media-server/docker-compose.yml
```

And copy all the containers you want to use under **services**. Remember to add the **version** string just like adguard home compose file.

## Configuring the docker stack

Start the containers using the same command we used to start the adguard home container.

```
docker-compose up -d
```

## Jackett

Navigate to `YOUR_SERVER_IP:9117`

Add a few indexers to jackett using the “add indexer” button. You can see the indexers I use in the image below.

​

[Indexers](https://lemmy.dbzer0.com/pictrs/image/6690e52f-a843-4551-ad66-04c2b34b889e.png)

## Qbittorrent

Navigate to `YOUR_SERVER_IP:8080`

The default username is `admin` and password `adminadmin`. You can change the user and password by going to `Tools → Options → WebUI`

Change “Default Save Path” in WebUI section to `/data/torrents/` and “Keep incomplete torrents in” to `/data/torrents/incomplete/`

Create categories by right clicking on sidebar under category. Type category as `TV` and path as `tv`. Path needs to be same as the folder you created to store your media. Similarly for movies type `Movies` as category and path as `movies`. This will enable to automatically move the media to its correct folder.

## Sonarr

Navigate to `YOUR_SERVER_IP:8989`

- Under “Download Clients” add qbittorrent. Enter the host as `YOUR_SERVER_IP` port as `**8080`,\*\* and the username and password you used for qbittorrent. In category type `TV` (or whatever you selected as **category** **name**(not path) on qbittorent). Test the connection and then save.
- Under indexers, for each indexer you added in Jackett
- Click on add button
- Select Torzab
- Copy the tornzab feed for the indexer from jackett
- Copy the api key from jackett
- Select the categories you want
- Test and save
- Under general, define the root folder as `/data/media/tv`

> Repeat this process for Radarr, Lidarr and readarr.

Use `/data/media/movies` as root for Radarr and so on.

> The setup for ombi/overseerr is super simple. Just hit the url and follow the on screen instructions.

## Bazarr

Navigate to `YOUR_SERVER_IP:6767`

Go to settings and then sonarr. Enter the host as `YOUR_SERVER_IP` port as `8989`. Copy the api key from sonarr settings→general.

Similarly for radarr, enter the host as `YOUR_SERVER_IP` port as `7878`. Copy the api key from radarr settings→general.

## Jellyfin

Go to `YOUR_SERVER_IP:8096`

- Add all the libraries by selecting content type and then giving a name for that library. Select the particular library location from `/data/media`. Repeat this for movies, tv, music, books and audiobooks.
- Go to dashboard→playback, and enable transcoding by selecting as `VAAPI` and enter the device as `/dev/dri/renderD128`

Monitor GPU usage while playing content using

```
sudo intel_gpu_top
```

## Heimdall

Navigate to `YOUR_SERVER_IP:8090`

Setup all the services you use so you don’t need to remember the ports like I showed in the first screenshot.

## Updating docker images

With docker compose updates are very easy.

- Navigate to the compose file directory `~/server/compose/media-server`.
- Then `docker-compose pull` to download the latest images.
- And finally `docker-compose up -d` to use the latest images.
- Remove old images by `docker system prune -a`

## What’s next

- You can setup VPN if torrents are blocked by your ISP/Country. I wanted to keep this guide simple and I don’t use VPN for my server, so I have left out the VPN part.
- You can read about port forwarding to access your server over the internet.

> I would recommend prowlarr instead of jackett for indexer management, and pihole as at least an additional blocking service but in reality it’s really all you need for use at home. I’d also strongly encourage use of a VPN on your *arr download services. I use a separate box to run Plex and then have my *arrs all running on their own VM inside if it to provide separation and allow be to more easily segregate the network traffic (as someone that doesn’t really know docker that well it “just works” for me. Also probably worth looking at how to store your media on an external target, it’s easy to quickly accumulate 10s of TBs of media and trying to store that all on the server locally is asking for trouble. Better to set everything up on a NAS to start.
> - [WxFisch](https://lemmy.world/post/6542543/4339288)
___
References:
- https://lemmy.world/post/6542543