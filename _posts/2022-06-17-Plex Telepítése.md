---
title: Plex Telepítése
date: 2022-06-17 12:44:00 +2
categories: [DOCKER]
tags: [docker-compose, docker]     # TAG names should always be lowercase
---

![L1p0 Inndustries Logo](https://i.imgur.com/JeX5nMc.png)

## Mi is a Plex?

A Plex egyetlen helyet biztosít Önnek, ahol megtalálhatja és elérheti az Ön számára fontos médiát.
A személyes médiától a saját szerverén, az ingyenes és igény szerinti filmeken és műsorokon, élő tévéadásokon, podcastokon és webes műsorokon át a zene streameléséig mindezt egyetlen alkalmazásban, bármilyen eszközön élvezheti.

> Röviden: A Plex egy médiaszerver, amin a letöltött filmeket,zenéket,sorozatokat nézhetjük
{: .prompt-info }

## Plex telepítése

### Első lépés

A container configját tartalmazó mappa létrehozása:

```bash
cd /dockerconfig
mkdir plex
cd plex
mkdir config
```

### Második lépés

A docker-compose fájl létrehozása:

```shell
cd /dockerconfig/plex
touch docker-compose.yml
```

A `/dockerconfig/plex/docker-compose.yml`{: .filepath } fájlba:

```yaml
---
version: "2.1"
services:
  plex:
    image: lscr.io/linuxserver/plex:latest
    container_name: plex
    network_mode: host
    environment:
      - PUID=1000
      - PGID=1000
      - VERSION=docker
      - PLEX_CLAIM= #optional
    volumes:
      - /dockerconfig/plex/config:/config
      - /hdd/series:/series
      - /hdd/plex:/movies
      - /hdd/preroll:/preroll
    restart: unless-stopped
```

> A **host** network mode szükséges a container működéséhez!!
{: .prompt-warning }


## Harmadik lépés

Ezután már csak el kell indítani a containert a `docker-compose.yml`{: .filepath} fájlból:

```shell
cd /dockerconfig/plex
docker-compose up -d --force-recreate
```

Amikor végzett már el is érjük az új containert a weboldalon:
> http://host-ip:32400
