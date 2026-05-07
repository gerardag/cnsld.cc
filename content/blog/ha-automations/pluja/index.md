---
title: Pluja
description: Per saber si plou i haig de guardar la roba
date: 2026-03-07T10:00:00Z
tags:
  - home-assistant
  - llar
---

Tinc moltes automatitzacions al meu Home Assistant, però poques de tant sencilles i tant útils com aquesta.

La roba estesa i la pluja no es porten bé. Estant a casa no sempre és fàcil veure que comença a ploure. Si tens les persianes abaixades, és de nit i estem veient com en Harry Potter lluita contra un Troll a les masmorres, doncs ja pot ploure ja, que nosaltres ni ho sabem. Així que necessitem alguna cosa que ens avisi.

## Com funciona

Gràcies a  [Home Assistant](https://home-assistant.io) i una [estació meteorològica](https://www.ecowitt.com/shop/goodsDetail/372) a casa, l'automatisme és ben senzill: quan el sensor detecta pluja, es dispara el trigger de l'automatisme i mira si estem a casa i  que no haguem anat a dormir. Aleshores envia una notificació tant al meu mòbil com al de la meva parella, avisant-nos que ha començat a ploure i que cal treure la roba (si en tenim d'estesa).

## L'automatisme

```YAML
alias: Pluja - Avisar quan comença a ploure
description: Envia una notificació quan comença a ploure mentre hi ha roba estesa.
triggers:
  - type: precipitation_intensity
    device_id: 82fec48a0c2340a99bea61eca1e1c97b
    entity_id: 4f74a9523107a881573aef1103bb6166
    domain: sensor
    trigger: device
    above: 0
conditions:
  - condition: state
    entity_id: binary_sensor.familia
    state:
      - "on"
  - condition: time
    after: "08:00:00"
    before: "23:59:59"
  - condition: state
    entity_id: input_boolean.avis_de_pluja
    state:
      - "off"
actions:
  - action: script.notificacions_sistema_unificat
    metadata: {}
    data:
      tipo: familia_casa
      title: 🌧️ Ei, que plou!
      message: >-
        Ha començat a ploure! ☔👕 Mira si hi ha roba estesa abans no es torni a
        rentar sola! 😆
      tag: pluja-inici
  - action: input_boolean.turn_on
    metadata: {}
    target:
      entity_id: input_boolean.avis_de_pluja
    data: {}
mode: single

```


La notificació arriba en qüestió de segons des que comença a ploure i gràcies a això ens hem estalviat haver de tornar a posar la roba de nou a la rentadora, estalviant aigua i energia.

## Condicions

Ho tinc que només ens avisi un cop al dia perquè no hi veig la necessitat que si durant el dia comença a ploure i para i torna a començar i a parar em vagi avisant cada vegada (en el nostre cas), perquè amb una sola alerta ja és suficient per tapar la roba.

També ho tenim limitat entres hores, perquè més tard de les 12 de la nit normalment ja estam al llit i no volem que ens espanti l'alerta. Si plou, doncs què hi farem. I el mateix al matí, si plou abans doncs mala sort.
