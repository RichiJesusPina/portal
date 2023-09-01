+++
date = "2022-05-23"
title = "Canigó. Actualització archetype 1.7.5 i plugin Eclipse 1.8.5"
description = "S'ha publicat una nova versió de l'archetype i del plugin de l'eclipse de Canigó per a generar projectes amb Canigó 3.6.5"
sections = ["Notícies", "home"]
categories = ["canigo"]
key = "JUNY2022"
+++

## Introducció

**L'arquetipus és una eina Maven que facilita la creació de projectes seguint una plantilla**. Utilitzant l'arquetipus i
executant un _goal_ de maven obtenim un projecte Canigó amb una base preconfigurada preparada per a incorporar-li noves
funcionalitats.

**El _plugin_ és un connector desenvolupat específicament per a l'IDE Eclipse que permet afegir-hi menús contextuals
per a la creació de projectes Canigó fent ús de l'arquetipus de Maven**, a més d'afegir mòduls a un projecte creat i
amb una preconfiguració donada. Així, usant l'Eclipse, podem crear un projecte i afegir-l'hi els mòduls necessaris
obtenint un projecte preconfigurat per al marc de treball Canigó i estalviant temps als desenvolupadors.

S'ha actualitzat l'arquetipus i el plugin de Canigó per a **generar projectes amb Canigó 3.6.5**.

Podeu consultar l'abast complet de la nova versió al
[Llistat de canvis](/plataformes/canigo/documentacio-per-versions/3.6LTS/3.6.5/llistat-de-canvis/).

## Novetats _archetype_

La versió 1.7.5 de l'arquetipus de Canigó incorpora les següents novetats:

- Creació de projectes amb Canigó 3.6.5

## Novetats _plugin_ Eclipse

La nova versió 1.8.5 del _plugin_ fa servir la versió 1.7.5 de l'arquetipus per a generar projectes amb Canigó 3.6.5.
Per a instal·lar o actualitzar aquesta versió del _plugin_ serà necessari seguir els passos descrits a la secció
"Instal·lació" de la documentació del [plugin de Canigó](/plataformes/canigo/entorn-de-desenvolupament/plugin-eclipse/).

Cal destacar que per a utilitzar el plugin a partir de la versió 1.8.0, és necessari iniciar l'eclipse amb la màquina
virtual de Java versió 11.

## Documentació addicional

Podeu consultar la notícia de la publicació de la [versió 3.6.5 de Canigó]
(/noticies/2022-05-23-CAN-actualitzacio-canigo-3_6_5/).