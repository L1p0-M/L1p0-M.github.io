---
title: Hardware
date: 2022-06-12 13:28:40 +2
categories: [HARDWARE]
tags: [hardware, raspberrypi]     # TAG names should always be lowercase
---

![L1p0 Inndustries Logo](https://i.imgur.com/JeX5nMc.png)

Ebben a postban a Homelab-em hardwerjeiről fogok írni,amin a docker container-jeim futnak...

## Gépek

A **szervergépeim**:

- Raspberry PI4 4GB (Docker szerver)
- Raspberry PI4 2GB (Home Assistant)

Ezek a Single Board Computer-ek a kevés fogyasztásuk,valamint a kevés "zaj" miatt nagyon jók!

## Tárhely

> De mivel SD Kártyáról nem lehet Plex szervert futattni,ezért külső tárhelyre van szükség!
{: .prompt-tip }

A Plex szerveremhez(Docker) a tárhelyet egy **WD 1TB Külső Merevlemez** biztosítja, erre ~250db HD Film fér.
De ezzel még mindíg ott van az SD Kártya meghibásodásának lehetősége,ami a sok logolástól bekövetkezhet, és be is fog.
Ezért mind a két szerverem egy-egy **Kingston 120GB SSD**-vel van ellátva,egy-egy sata-usb adapterrel.