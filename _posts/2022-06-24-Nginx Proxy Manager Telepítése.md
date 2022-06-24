---
title: Nginx Proxy Manager Telepítése
date: 2022-06-24 13:38:00 +2
categories: [DOCKER]
tags: [docker-compose, docker]     # TAG names should always be lowercase
---

![L1p0 Industries Logo](https://i.imgur.com/JeX5nMc.png)

## Mi is az Nginx Proxy Manager?

The Nginx proxy manager (NPM) is a reverse proxy management system running on Docker.
NPM is based on an Nginx server and provides users with a clean, efficient, and beautiful web interface for easier management.

> Röviden: Az NPM egy UI-el ellátott Reverse Proxy
{: .prompt-info }

### Mi az a Reverse Proxy?
A Reverse Proxy egy szerver ami webszerverek előtt ül és átirányítja a klienseket a webszerverekhez.
![Reverse Proxy Flow](/assets/img/reverse_proxy.webp)

Az ábrán látható módon a kliens el akar jutni "example.com" domain-el jelzett webszerverhez és a Reverse Proxy átirányítja a megfelelő szerverhez.

#### Mi kell ahhoz, hogy működjön?

A Reverse Proxy működéséhez csak meg kell nyitni a Routerünkön a **443/tcp** és a **80/tcp** portokat és a Reverse Proxynk ip-jéhez irányítanunk.

#### Miért jó nekünk?

A Reverse Proxy segítségével **nem** kell megnyitnunk portokat minden szerverhez, amit távolról is el szeretnénk érni.
Így az adott domain-re vagy subdomain-re jött kérést a Reverse Proxy fogja továbbítani a webszerverek felé.

## Nginx Proxy Manager telepítése

### Első lépés

A container adatait tartalmazó mappa és a docker hálózat létrehozása:

```bash
cd /dockerconfig
mkdir nginx
mkdir letsencrypt
cd nginx
mkdir data
```

```bash
docker network create reverseproxy
```

### Második lépés

A docker-compose fájl létrehozása:

```shell
cd /dockerconfig/nginx
touch docker-compose.yml
```

A `/dockerconfig/nginx/docker-compose.yml`{: .filepath } fájlba:

```yaml
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm"
      DB_MYSQL_NAME: "npm"
    volumes:
      - /dockerconfig/nginx/data:/data
      - /dockerconfig/letsencrypt:/etc/letsencrypt
  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
    volumes:
      - /dockerconfig/nginx/mysql:/var/lib/mysql
networks:
  default:
    name: "reverseproxy"
    external: true
```

## Harmadik lépés

Ezután már csak el kell indítani a containert a `docker-compose.yml`{: .filepath} fájlból:

```shell
cd /dockerconfig/nginx
docker-compose up -d --force-recreate
```

Amikor végzett már el is érjük az új containert a weboldalon:
> http://host-ip:81

Alapértelmezett Admin felhasználó:

> **Email:    admin@example.com**
>
> **Password: changeme**
