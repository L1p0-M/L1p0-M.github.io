---
title: ESPHome, Glances Telepítése
date: 2022-07-17 13:18:20 +2
categories: [DOCKER]
tags: [docker-compose, docker]     # TAG names should always be lowercase
---

![L1p0 Industries Logo](https://i.imgur.com/JeX5nMc.png)

## Mi is az ESPHome?

ESPHome is a system to control your ESP8266/ESP32 by simple yet powerful configuration files and control them remotely through Home Automation systems.

> Röviden: Az ESPHome egy ESP8266/ESP32 programozó software
{: .prompt-info }

## Mi is a Glances?

Glances is a cross-platform system monitoring tool written in Python.

> Röviden: A Glances egy system monitorozó eszköz
{: .prompt-info }

## ESPHome telepítése

### Első lépés

A container adatait tartalmazó mappa létrehozása:

```bash
cd /dockerconfig
mkdir esphome
cd esphome
mkdir config
mkdir cache
mkdir mdns repeater
```

### Második lépés
#### ESPHome
A docker-compose fájl létrehozása:

```shell
cd /dockerconfig/esphome
touch docker-compose.yml
```

A `/dockerconfig/esphome/docker-compose.yml`{: .filepath } fájlba:

```yaml
version: '2.1'
services:
  esphome:
    container_name: Esphome
    image: esphome/esphome
    volumes:
      - /dockerconfig/esphome/config:/config
      - /etc/localtime:/etc/localtime:ro
      - /dockerconfig/esphome/cache:/cache
    restart: unless-stopped
    privileged: true
    environment:
      - VIRTUAL_HOST=esphome.local.exampledomain.com #Ezt arra a subdomainre kell írni amit az esphomehoz akarunk használni!!
      - PASSWORD=mypassword
      - USERNAME=myusername
networks:
  default:
    name: reverseproxy
    external: true
```

> Az ESPHome-hoz szükségünk van egy mdns repeater-re is az online állapot érzékeléséhez!
{: .prompt-warning }
#### MDNS Repeater

A docker-compose fájl létrehozása:

```shell
cd /dockerconfig/esphome/"mdns repeater"
touch docker-compose.yml
```

A `/dockerconfig/esphome/"mdns repeater"/docker-compose.yml`{: .filepath } fájlba:

```yaml
version: '3'
services:
  mdns-repeater:
    image: monstrenyatko/mdns-repeater
    container_name: mdns-repeater
    restart: unless-stopped
    command: mdns-repeater-app -f eth0 br-020e83007fb1 #például mdns-repeater-app -f eth0 br-abcdefghijk
    network_mode: "host"
```

> Itt a **"br-020e83007fb1"** a reverseproxy network, az **"eth0"** pedig a hálózat amin a szervergép van!
> A reverseproxy network nevét megkereshetjük a `docker network list` majd az `ifconfig` parancsal!
{: .prompt-info }

## Harmadik lépés

Ezután már csak el kell indítani a containereket a `docker-compose.yml`{: .filepath} fájlból:

```shell
cd /dockerconfig/esphome
docker-compose up -d --force-recreate
cd /dockerconfig/esphome/"mdns repeater"
docker-compose up -d --force-recreate
```

Amikor végzett már csak a reverseproxy-ban kell a 6052-es portra domaint mapelnünk!

## Glances telepítése

### Első lépés

A container adatait tartalmazó mappa létrehozása:

```bash
cd /dockerconfig
mkdir glances
```

### Második lépés
A docker-compose fájl létrehozása:

```shell
cd /dockerconfig/glances
touch docker-compose.yml
```

A `/dockerconfig/glances/docker-compose.yml`{: .filepath } fájlba:

```yaml
---
version: "2.1"
services:
  glances:
    image: nicolargo/glances:latest
    container_name: Glances
    environment:
      - GLANCES_OPT=-w
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    restart: unless-stopped
networks:
  default:
    name: reverseproxy
    external: true
```

## Harmadik lépés

Ezután már csak el kell indítani a containert a `docker-compose.yml`{: .filepath} fájlból:

```shell
cd /dockerconfig/glances
docker-compose up -d --force-recreate
```

Amikor végzett már csak a reverseproxy-ban kell a 61208-as portra domaint mapelnünk!!