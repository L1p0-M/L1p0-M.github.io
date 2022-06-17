---
title: Portainer Telepítése
date: 2022-06-17 09:13:00 +2
categories: [DOCKER]
tags: [docker-compose, docker]     # TAG names should always be lowercase
---

![L1p0 Inndustries Logo](https://i.imgur.com/JeX5nMc.png)

## Mi is a Portainer?

Portainer is a lightweight management UI which allows you to easily manage your different Docker environments (Docker hosts or Swarm clusters).
Portainer is meant to be as simple to deploy as it is to use.

> Röviden: A Portainer egy Docker management UI
{: .prompt-info }

## Portainer telepítése

### Első lépés

A conainer adatait tartalmazó mappa létrehozása:

```bash
cd /dockerconfig
mkdir portainer
cd portainer
mkdir data
```

### Második lépés

A docker-compose fájl létrehozása:

```shell
cd /dockerconfig/portainer
touch docker-compose.yml
```

A `/dockerconfig/portainer/docker-compose.yml`{: .filepath } fájlba:

```yaml
version: '3'

services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: always
    security_opt:
      - no-new-privileges:true
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /dockerconfig/portainer/data:/data
    ports:
      - 9000:9000
```

## Harmadik lépés

Ezután már csak el kell indítani a containert a `docker-compose.yml`{: .filepath} fájlból:

```shell
cd /dockerconfig/portainer
docker-compose up -d --force-recreate
```

Amikor végzett már el is érjük az új containert a weboldalon:
> http://<host-ip>:9000
