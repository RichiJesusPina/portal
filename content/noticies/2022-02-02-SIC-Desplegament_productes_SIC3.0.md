+++
date        = "2022-02-02"
title       = "SIC. Desplegament de productes al SIC 3.0"
description = "El SIC 3.0 permet dur a terme el desplegament en contenidors de productes a partir d’imatges existents en registres públics."
sections    = ["Notícies", "home"]
categories  = ["SIC"]
key         = "FEBRER2022"
+++

## Introducció

El **Servei d'Integració Contínua és un servei a disposició dels proveïdors d'aplicacions per a automatitzar el desplegament
de les aplicacions**. Fins ara, les imatges a desplegar calia construir-les en temps d'execució a partir del seu DockerFile
sense permetre, per tant, el desplegament d'imatges públiques empaquetades externament.

## Novetats

El **SIC 3.0 passa a permetre el desplegament en contenidors de productes, a partir d'imatges existents en registres públics**.
Aquestes imatges no cal construir-les via SIC i han de poder ser executades dins una plataforma de contenidors mitjançant
determinades configuracions (secrets, configMaps, variables d'entorn i altres).
Per al desplegament d'imatges públiques de productes, caldrà configurar el registre extern, la imatge del producte i la
versió corresponent, de manera que la pipeline pugui dur a terme correctament la descàrrega de la imatge del registre
públic, fer un tag i publicar la imatge dins el projecte propi de l'aplicació al registre corporatiu d'imatges (Harbor).

Amb l’objectiu que els usuaris sàpiguen com s’ha de configurar i quin serà el funcionament, s’ha adaptat la documentació i s’han
incorporat exemples:

- [Autoservei de pipelines](/plataformes/sic/serveis/sic30-serveis/autoservei-pipelines/)
- [Com construir el fitxer ACA](/sic30-guies/fitxer-aca/)
- [Exemple fitxer ACA](/related/sic/3.0/aca_const_despl_external_product_openshift.yml)

<br/><br/>
Si teniu qualsevol dubte o problema podeu revisar les [**Preguntes Freqüents**] (/sic/faq) o utilitzar els canals de [**Suport**] (/sic/suport).