+++
date        = "2021-11-15"
title       = "SIC. Expiració punts d'aprovació pipelines SIC 2.0"
description = "A partir del 18/11/2021 els punts d'aprovació de les pipelines del SIC 2.0 expiren en un termini màxim de 30 dies."
sections    = ["Notícies", "home"]
categories  = ["SIC"]
key         = "DESEMBRE2021"
+++

## Introducció

El **Servei d'Integració Contínua és un servei a disposició dels proveïdors d'aplicacions per a automatitzar el desplegament
de les aplicacions**. En aquest sentit, les pipelines de desplegament incorporen una sèrie de punts manuals d'aprovació per part
de l'usuari que permeten decidir el moment en què es vol iniciar el desplegament en un determinat entorn, així com confirmar
que el desplegament i les corresponents verificacions han estat satisfactòries.

## Novetats

Amb l'objectiu de controlar i limitar la concurrència i l'existència de tasques aturades en punts manuals d'aprovació, **a partir del
18/11/2021 s'aplica un criteri d'expiració de manera que aquests punts manuals d'aprovació de les pipelines SIC 2.0
expiren en un termini màxim de 30 dies**. Fins a aquesta data, al SIC 2.0 els punts d'aprovació no expiraven mai, la qual cosa
implicava dur a terme una tasca periòdica de manteniment per a anar fent la neteja necessària. En canvi, al SIC 3.0 ja es va preveure
aquesta necessitat i, per tant, es va aplicar d'entrada aquest mateix criteri d'expiració.


<br/>
Per a més informació:

- [Servei d'Integració Contínua](/plataformes/sic/serveis/sic20-serveis/ci/)

<br/><br/>
Si teniu qualsevol dubte o problema podeu revisar les [**Preguntes Freqüents**] (/sic/faq) o utilitzar els canals de [**Suport**] (/sic/suport).