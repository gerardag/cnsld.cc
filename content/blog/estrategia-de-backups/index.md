---
title: Estratègia de backups
Description: Com gestiono les còpies de seguretat
date: 2025-05-27T00:00:00Z
draft: false
---

El gran tema.

Totes les dades de casa estan centralitzades al NAS i on els diferents serveis hi accedeixen.

## TrueNAS

Al NAS és on hi ha tota la informació: fotos, documents, backups. I és ell qui gestiona l'accés dels serveis a les dades que els corresponen:
- **Immich**: fotos i vídeos
- **Nextcloud**: documents
- **Home Assistant**: backups de la configuració de tot el sistema (no guardo les còpies dels plugins)
- **Proxmox**: fa còpies de seguretat de les VM

Com que TrueNAS té un sistema per fer gestionar còpies i pujar-les a algun servei cloud, doncs és el que faig servir. Perquè complicar-me si ja n'hi ha un, i que per mi ja serveix.

## Com i on faig les còpies

El "com" és fàcil, amb Data Protection i Cloud Sync Tasks de TrueNAS. Faig còpies un cop al dia de les diferents coses que hi tinc:

- **Fotos**: 3AM
- **Documents** i HA: 4AM

I l'"on" encara més fàcil, a un StorageBox de Hetzner que em costa 3.58€/mes per 1TB d'emmagatzematge. On al seu temps hi faig snapshots, per si les mosques, amb el seu sistema.

## Altres còpies

Tinc un altre sistema de còpies per les configuracions dels serveis que tinc a casa i que funcionen d'una manera diferent.

Pels serveis tinc un cron que em va passar el meu amic [Aleix](https://aleix.cloud), on cada matinada fa una còpia amb restic i a la puja a Borgbase, fent ús del free tier. De moment ja em serveix i quan em quedi sense espai o vulgui canviar-ho doncs ja veurem què fem.