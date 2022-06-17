---
title: Automatizált Médiaszerver
date: 2022-06-17 14:04:00 +2
categories: [DOCKER]
tags: [docker-compose, docker]     # TAG names should always be lowercase
---

![L1p0 Inndustries Logo](https://i.imgur.com/JeX5nMc.png)

# Jackett, Radarr, qBittorrent, Ombi

## Mi is a Jackett?

Jackett is a single repository of maintained indexer scraping & translation logic - removing the burden from other apps.

## Mi is a Radarr?

Radarr is an independent fork of Sonarr reworked for automatically downloading movies via Usenet and BitTorrent.

> Röviden: A Radarr egy automatikus film letöltő
{: .prompt-info }

## Mi is a qBittorrent?

Ahogy a nevéből is látható, a qBittorrent egy Torrent kliens.

## Mi is az Ombi?

Ombi is a self-hosted web application that automatically gives your shared Plex, Emby or Jellyfin users the ability to request content by themselves!
Ombi can be linked to multiple TV Show and Movie DVR tools to create a seamless end-to-end experience for your users.

> Röviden: Az Ombi egy Sorozatok,Filmek kérésére szolgáló weboldal
{: .prompt-info }

## Telepítés

### Első lépés

A containerek configját tartalmazó mappák létrehozása:

```shell
/
└── dockerconfig 
    ├── jackett
        └── config
    ├── radarr
        └── config	
    ├── qbittorrent
        └── config
    └── ombi
        └── config
```

Az "*rr" network létrehozása:

```bash
docker network create *rr
```

### Jackett telepítése

#### Első lépés

A docker-compose fájl létrehozása:

```shell
cd /dockerconfig/jackett
touch docker-compose.yml
```

A `/dockerconfig/jackett/docker-compose.yml`{: .filepath } fájlba:

```yaml
---
version: "2.1"
services:
  jackett:
    image: lscr.io/linuxserver/jackett:latest
    container_name: jackett
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Budapest
    volumes:
      - /dockerconfig/jackett/config:/config
      - /hdd/Movies:/downloads
    ports:
      - 9117:9117
    restart: unless-stopped
networks:
  default:
    name: "*rr"
    external: true
```

#### Második lépés

Ezután már csak el kell indítani a containert a `docker-compose.yml`{: .filepath} fájlból:

```shell
cd /dockerconfig/jackett
docker-compose up -d --force-recreate
```

Amikor végzett már el is érjük az új containert a weboldalon:
> http://host-ip:9117

### Radarr telepítése

#### Első lépés

A docker-compose fájl létrehozása:

```shell
cd /dockerconfig/radarr
touch docker-compose.yml
```

A `/dockerconfig/radarr/docker-compose.yml`{: .filepath } fájlba:

```yaml
---
version: "2.1"
services:
  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Budapest
      - DOCKER_MODS=ghcr.io/gilbn/theme.park:radarr
      - TP_COMMUNITY_THEME=true
      - TP_THEME=space
    volumes:
      - /dockerconfig/radarr/config:/config
      - /hdd:/hdd
    ports:
      - 7878:7878
    restart: unless-stopped
networks:
  default:
    name: "*rr"
    external: true
```

#### Második lépés

Ezután már csak el kell indítani a containert a `docker-compose.yml`{: .filepath} fájlból:

```shell
cd /dockerconfig/radarr
docker-compose up -d --force-recreate
```

Amikor végzett már el is érjük az új containert a weboldalon:
> http://host-ip:7878

### qBittorrent telepítése

#### Első lépés

A docker-compose fájl létrehozása:

```shell
cd /dockerconfig/qbittorrent
touch docker-compose.yml
```

A `/dockerconfig/qbittorrent/docker-compose.yml`{: .filepath } fájlba:

```yaml
---
version: "2.1"
services:
  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Budapest
      - WEBUI_PORT=8300
    volumes:
      - /dockerconfig/qbittorrent/config:/config
      - /hdd:/hdd
    ports:
      - 8300:8300
      - 51000:51000
      - 51000:51000/udp
    restart: unless-stopped
```

#### Második lépés

Ezután már csak el kell indítani a containert a `docker-compose.yml`{: .filepath} fájlból:

```shell
cd /dockerconfig/qbittorrent
docker-compose up -d --force-recreate
```

Amikor végzett már el is érjük az új containert a weboldalon:
> http://host-ip:8300

### Ombi telepítése

#### Első lépés

A docker-compose fájl létrehozása:

```shell
cd /dockerconfig/ombi
touch docker-compose.yml
```

A `/dockerconfig/ombi/docker-compose.yml`{: .filepath } fájlba:

```yaml
---
version: "2.1"
services:
  ombi:
    image: lscr.io/linuxserver/ombi:latest
    container_name: ombi
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Budapest
    volumes:
      - /dockerconfig/ombi/config:/config
    restart: unless-stopped
networks:
  default:
    name: "reverseproxy"
    external: true
```

> Mivel azt szeretnénk,hogy csak a külső URL-ről legyen elérhető ezért az Nginx Proxy Manager networkre kapcsoljuk!
{: .prompt-warning }

#### Második lépés

Ezután el kell indítani a containert a `docker-compose.yml`{: .filepath} fájlból:

```shell
cd /dockerconfig/ombi
docker-compose up -d --force-recreate
```

#### Harmadik lépés

Mivel portot nem map-eltünk a hostra,ezért az Nginx Proxy Managerben kell beállítanunk a hozzá tartozó domaint/subdomaint:
![NPM Config](/assets/img/NPM_Ombi.png)
> Mivel a containert az Nginx Proxy Manager-rel egy networkre kapcsoltuk, containernév alapján be tudja azonosítani
{: .prompt-info }

Ezután már el is érjük az új containert a weboldalon:
> http(s)://ombi.exampledomain.hu


