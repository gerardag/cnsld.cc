---
title: "qbit-voter"
date: 2026-05-05T10:00:00+02:00
description: "Perquè s'ha de ser agraït."
tags: ["homelab", "eines"]
---

Quan descarrego un torrent (ISOs de Linux), el mínim que puc fer és ser agraït i donar les gràcies. Un vot, un "thank you", el que sigui. És el que fa que les comunitats segueixin funcionant.

El problema és que fer-ho és un pal.

## El problema

Cada torrent descarregat porta al comentari una URL per votar-lo i donar-li les gràcies. Per fer-ho, fins ara, havies de seguir aquests passos:

1. Obrir qBittorrent.
2. Buscar el torrent.
3. Mirar les propietats per trobar la URL del comentari.
4. Copiar-la, obrir el navegador, enganxar-la.
5. Votar.
6. Repetir per cada torrent pendent.

Amb un o dos torrents no és res. Però quan se n'acumulen uns quants, doncs no ho fas. I és una llàstima perquè la gent que comparteix coneixement s'ho mereix.

## La solució

He fet [qbit-voter](https://github.com/gerardag/qbit-voter), una app web que es connecta al qBittorrent i et llista tots els torrents que encara no has agraït (en el meu cas els que no tenen l'etiqueta "Liked"). Per cada un et mostra el nom i un botó per obrir directament la pàgina de votació. Un cop has votat, li dones a "Fet" i l'app etiqueta el torrent perquè no torni a sortir.

Bàsicament passa de ser 6 passos per torrent a ser 2 clics.

## Com funciona

- Llista els torrents que **no tenen l'etiqueta** "Liked" (pots configurar la que tu vulguis).
- Extreu la **URL de votació** del camp comentari de cada torrent.
- Et permet **obrir la URL** i **marcar-lo com a votat** directament des de l'app.
- Suporta **català i anglès** (obert a que la gent pugui contribuir amb l'idioma que vulguin).

## Desplegament

És una imatge Docker lleugera (Node.js Alpine) i que per connectar-se al qBittorrent només necessita la URL i les credencials que es configuren a través de la UI. Així que el desplegament és tan senzill com això:

```bash
docker run -d \
  -p 3000:3000 \
  ghcr.io/gerardag/qbit-voter:latest
```

Si tens Kubernetes, Docker Compose o el que sigui, funciona igual de bé.

El projecte és [open source](https://github.com/gerardag/qbit-voter) i les contribucions són més que benvingudes. Espero que sigui útil per a la comunitat i que ajudi a fomentar la gratitud entre els usuaris de torrents. Al cap i a la fi, compartir és cuidar 😉.