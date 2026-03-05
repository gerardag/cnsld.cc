---
title: 'El meu homelab'
Description: 'Els ferros.'
date: 2025-05-10T10:00:00Z
draft: false
tags: ["homelab"]
---

Fa uns anys que tinc aparells per casa que fan cosetes: des d'una RPi Model B de fa anys, passant per una RPi 3 Model A, i una RPi 3 de 4GB, entre altres. I del que inicialment era només Home Assistant amb uns quants plugins per tenir algun servei extra (Vaultwarden, Adguard i algun més) he acabat amb 2 mini-pcs amb proxmox, màquines virtuals i serveis desplegats amb k8s i gestionat amb Terraform.

I per tenir tots els serveis en funcionament tinc actualment aquest setup:

- Minsforum MS-01 (i5 12600h) amb 64GB de RAM i 4 discs interns SSD i un HDD de 5TB extern.
- Chuwi Larkbox amb 12GB de RAM i un SSD.

## Proxmox

Els dos mini-pcs tenen Proxmox instal·lat i són nodes del mateix datacenter (si, me'n falta un per tenir quòrum 🥺).

### Minisforum MS-01

És el meu node "principal". És on hi ha tots els serveis que utilitzo diàriament i on hi ha el meu node de k8s. Té 4 discs SSD distribuits de la següent manera:

- 1 disc de 512GB amb el sistema operatiu (Proxmox)
- 1 disc de 512GB on tinc els backups de les màquines virtuals
- 2 discs de 2TB en RAID 1 per al TrueNAS

### Chuwi Larkbox

És el meu node "secundari". Hi ha una màquina virtual amb Home Assistant i un container LXC amb el Adguard.

---

I de moment això és tot. Si el vaig actualitzant o ampliant aniré comentant aquestes millores dins del post.

