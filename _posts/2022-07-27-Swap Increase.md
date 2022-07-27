---
title: Raspberry PI/Linux SWAP tárhely növelése
date: 2022-07-27 09:58:40 +2
categories: [LINUX]
tags: [linux]     # TAG names should always be lowercase
---

![L1p0 Industries Logo](https://i.imgur.com/JeX5nMc.png)
## Mi is az a SWAP Tárhely?

SWAP memory is basically parts of memory from the RAM (Random Access Memory) that enables an operating system to provide more memory to a running application or process than is available in physical random access memory (RAM).
So if the physical memory (RAM) is full, we can use SWAP partition for extra memory resources.
It is useful if we have low memory on our machine.

> Röviden: A SWAP Tárhely egy RAM-ot "megnövelő" eszköz,amivel az alkalmazásokat a rendszer tárhelybe tudjuk cache-elni a RAM helyett.
{: .prompt-info }

## Hogyan növeljük meg Linuxon?

### Első lépés: A SWAP leállítása

```bash
sudo dphys-swapfile swapoff
```

### Második lépés: A SWAP fájl megváltoztatása

```bash
sudo nano /etc/dphys-swapfile
```

A fájlban a `CONF_SWAPSIZE:`-ot kell megváltoztatni a kívánt swap értékre MB-ban kifejezve:

```bash
CONF_SWAPSIZE=2048
```
Ez így például 2GB SWAP-et jelent!

### Harmadik lépés: A SWAP fájl létrehozása és elindítása

```bash
sudo dphys-swapfile setup
```
Ezzel a SWAP fájl elkészült,már csak el kell indítani:

```bash
sudo dphys-swapfile swapon
```

![Glances](/assets/img/swap.png){: width="691" height="144" }

