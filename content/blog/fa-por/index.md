---
title: Fa por?
Description: Per saber si hi haurà espants.
date: 2026-02-25T10:00:00Z
---

No m'han agradat, no m'agraden i no crec que en un futur m'agradin les pel·lícules o les sèries de por.

Normalment quan tinc dubtes del contingut que vull veure, el que faig és entrar a Google i buscar el títol i així veig els gèneres. El problema? Moltes vegades no queda prou clar només amb els gèneres, tant per si en fa com per si no en fa.

> Zombieland està categoritzada com a terror, i no és el mateix que IT, però amb matisos.

Aleshores sempre haig de fer una mica d'investigació per saber si estaré o no tranquil veient un contingut. I vaig pensar, per què no faig una petita aplicació que donat un títol em digui ràpidament què em puc trobar? Farà por? Té algun susto? No és de susto però genera tensió, etc?

Doncs amb això he fet [Fa por?](https://fapor.cnsld.cc). Que és tant simple com: busques un títol i et diu què et trobaràs.

Per saber-ho faig servir la api de TMDB per recuperear la informació de gèneres i keywords de l'item per determinar quin tipus de contingut hi trobarem.

De moment ja és funcional i el que em retorna és bastant fiable, tot i que alguns items encara no els categoritza del tot bé (sobretot perquè diu que són de terror i no és el cas).

El següents passos són anar afinant els resultats i fent millores que aportin valor a l'usuari però sempre mantenint la màxima simplicitat.