+++
date        = "2021-05-01"
title       = "Arquitectura de Referència de Microserveis"
description = "Arquitectura de referència orientada al desplegament d'aplicacions en microserveis i microfrontends."
sections    = ["drafts"]
weight      = 1
+++

# Arquitectura de referència de microserveis

## Introducció

Davant la ràpida evolució dels estils arquitecturals, s'ha identificat la necessitat d’establir una arquitectura de referència per aquelles aplicacions que opten per arquitectures altament distribuïdes tant a nivell lògic com a nivell físic. L'arquitectura presentada en aquesta pàgina s'orienta al desplegament d'aplicacions en microserveis i microfrontends de forma que un arquitecte la pugui usar com a punt de partida per a realitzar el disseny d'una solució.

### Conceptualització de l’arquitectura de referència

L'arquitectura de referència ha estat conceptualitzada en base a principis, criteris, models d'ús i una metodologia de disseny de més abstracte a més detallat. En la il·lustració es mostren els conceptes treballats:

![Motivacions](/images/armicro/img1.png)

El primer punt a treballar han estat els  **principis de l’arquitectura de referència.** Aquests principis són els valors fonamentals que s’apliquen en l’arquitectura i que guien els models d’ús. Aquests s'han explicitat en dos, el de disseny funcional i el de devops. El **Model d’ús de disseny funcional** proporciona guies i recomanacions per generar la vista funcional i informacional del disseny d’una solució, mentre que el **Model d’ús de devops** proporciona guies i recomanacions per realitzar la vista de desenvolupament i operació del disseny d’una solució. Un cop realitzats els models d'ús, es creen els dissenys des del pla més abstracte - conceptual - fins el més detallat - físic. En primer lloc, el **Disseny conceptual** identifica els components de l’arquitectura de referència des d’un punt de vista tecnològicament neutre, definint quines capacitats aporten a l’arquitectura. A continuació, el **Disseny lògic** aporta una realització tecnològica del disseny conceptual i explicita les tecnologies possibles a usar per realitzar cadascun dels components definits, que poden tenir múltiples realitzacions tecnològiques diferents. L'últim dels dissenys és el **Disseny físic,** que representa la implementació del disseny lògic sobre una arquitectura física concreta, per exemple, sobre on-premise, clouds públics, etc. De forma similar al cas anterior, un disseny lògic pot tenir vàries implementacions físiques, segons altres condicionants que pot tenir el sistema. Finalment, s'ha considerat convenient afegir un conjunt de **Criteris d’aplicabilitat,** que permeten fer una discriminació inicial de quines solucions poden ser candidates a usar l’arquitectura de referència i, per tant, treure’n benefici de les seves qualitats.

## Principis de l’arquitectura de referència

Els principis són els **valors fonamentals** que s’apliquen en l’arquitectura i que guien els models d’ús de disseny funcional i devops.

![Principis](/images/armicro/img4.png)

## Models d’ús de l'arquitectura

### Model d'ús del disseny funcional

L'objectiu del disseny funcional és la **distribució adequada del conjunt de funcionalitats de la solució en diferents blocs funcionals**. Els principis bàsics que han de guiar la descomposició funcional són: L'**alta cohesió** que per definició és que el codi que es modifica junt, que dona resposta a una funcionalitat concreta, ha d’estar arquitecturalment junt; i el **baix acoblament** que es produeix quan un bloc funcional està poc acoblat amb un altre, un canvi en un d'ells no implica canviar l'altre.

![DFuncional](/images/armicro/img4a.png)

### Model d'ús de DevOps

Per a cada bloc funcional el desenvolupament hauria de seguir un **patró BFF (Backend for frontend)** amb les següents condicions: a) S'hauria de crear un únic projecte per a cada bloc funcional, que contindrà tant la part front com la part back, b) la part front només es comunica amb la seva part back, de manera que la part front està acoblada amb la seva part back, i c) si cal, cada bloc funcional hauria de disposar de la seva pròpia base de dades.

![Condicionants](/images/armicro/img4b.png)

*(\*) En el cas en què un microservei d'un bloc funcional tingui requisits no funcionals particulars que diferencien o identifiquen dues o més parts diferenciades dins del microservei, es recomana dividir el microservei en diferents components (dins del mateix bloc funcional) per poder així aplicar la particularitat cadascun (p.e. escalat diferenciat).*

#### *Model de desplegament*

La capa back del bloc funcional es desplega en un recurs dedicat i la capa front es desplega en un directori d'un servei front global, que serveix les capes front de tots els blocs funcionals de l'aplicació.

#### *Model d’operacions*

El model d'operacions ha de tenir en compte que en els **serveis de monitorització, logging i observabilitat** s'han de crear dashboards independents per a cada bloc funcional. Tanmateix s'ha de tenir en compte les plantilles per als **serveis de gestió de configuració** dels components dels que es compon un bloc funcional s'emmagatzemen en el propi projecte. Els **serveis de control** per als blocs funcionals estan coberts per la pròpia infraestructura. I per millorar la disponibilitat i la resiliència dels blocs funcionals, en els serveis de control s'haurien d'implementar tant **healthiness i readiness checks** com el patró de **circuit breaker**.

## Arquitectura tècnica

### Disseny conceptual

El disseny conceptual identifica i descriu els seus components des d’un punt de vista neutre tecnològicament i s’ha dividit en els tres blocs: **execució**, **operació** i **desenvolupament**.

#### *Arquitectura d’execució*

Inclou els components necessaris per tal de poder executar els microserveis i microfrontends de la solució i que aquests interactuïn entre ells. El fet que l’arquitectura sigui altament distribuïda requereix l’existència de certs components que no són necessaris en arquitectures monolítiques. Aquests components són els que donen resposta als requeriments dels usuaris de negoci.

![Condicionants](/images/armicro/img15.png)

#### *Arquitectura d’operació*

Els components d’aquest bloc permeten que, un cop desplegada i en execució la solució, aquesta pugui ser supervisada i es pugui assegurar el seu nivell de servei. En arquitectures altament distribuïdes no és suficient una visió independent de cada artefacte de la solució sinó que calen visions agregades de monitorització, observabilitat, etc. Aquests components donen resposta als perfils d’operació de la solució.

![Condicionants](/images/armicro/img16.png)

#### *Arquitectura de desenvolupament*

Inclou els components per poder desenvolupar i desplegar els microfrontends i microserveis que composen la solució. Per treure màxim profit de l’arquitectura és necessari que els diferents artefactes de la solució tinguin un cicle de vida el més autònom possible. Aquests components donen servei als equips de desenvolupament de la solució.

![Condicionants](/images/armicro/img17.png)

### Disseny lògic i físic

El **disseny lògic** és una realització concreta del disseny conceptual especificant les tecnologies que poden resoldre cada servei i el **disseny físic** identifica i descriu els components que formen part o integren el sistema, i com aquests es configuren o interactuen. 


## Criteris d’aplicabilitat

L’ús de l’arquitectura aporta avantatges, especialment, relacionats amb la capacitat de lliurar canvis i evolucions de la solució més ràpidament als usuaris però també introdueix dificultats i complexitats que fan que la decisió sobre l’ús d’aquest tipus d’arquitectura sigui rellevant. És per aquest motiu que s’estableixen uns criteris d’aplicabilitat per ajudar a la presa de decisió sobre l’ús de l’arquitectura. Aquests criteris es divideixen en **motivacions** que poden fer que una arquitectura de microserveis pugui ser beneficiosa per a un sistema i **condicionants** que hauria de complir el sistema per a poder-la aplicar amb garanties d’èxit.

### Motivacions

Les motivacions serien aquelles circumstàncies o requeriments que impulsarien la decisió de crear una arquitectura basada en microserveis per a un sistema donat

![Motivacions](/images/armicro/img2.png)

### Condicionants

Per altra banda, l’ús d'una arquitectura de microserveis també implica uns condicionants que es poden classificar en diferents aspectes: solució, procés de construcció, equip i infraestructura de desplegament.

![Condicionants](/images/armicro/img3.png)

## Document de l'Arquitectura de Referència

El document complet en el qual es basa aquesta arquitectura de referència es pot descarregar com a fitxer PDF en ["Arquitectura de referència microserveis"](/related/armicro/ArquitecturaReferenciaMicro_V1.0.pdf).

## *Referències*

* [AZ021] Microservices on Azure – What Is Microservices | Microsoft Azure [Internet]. [citat 18 març 2021]. Disponible a: https://azure.microsoft.com/en-us/solutions/microservice-applications/

* [AZU21] Improving observability of your Kubernetes deployments with Azure Monitor for containers [Internet]. [citat 18 març 2021]. Disponible a: https://azure.microsoft.com/en-us/blog/improving-observability-of-your-kubernetes-deployments-with-azure-monitor-for-containers/

* [BRA13] Brandolini A. Introducing Event Storming [Internet]. Ziobrando’s Lair. 2013 [citat 18 març 2021]. Disponible a: http://ziobrando.blogspot.com/2013/11/introducing-event-storming.html

* [CTT21] Principis d’arquitectura de sistemes d’informació [Internet]. [citat 18 març 2021]. Disponible a: https://canigo.ctti.gencat.cat/arqctti/principis_arq/

* [EVA03] Evans E. Domain-driven design: tackling complexity in the heart of software. Boston, MA: Addison-Wesley; 2003.

* [HAR21] Harbor [Internet]. [citat 18 març 2021]. Disponible a: https://goharbor.io/

* [HON21] The Microservices Observability Problem [Internet]. Honeycomb. [citat 18 març 2021]. Disponible a: https://www.honeycomb.io/microservices/

* [NEW15] Newman S. Building microservices: designing fine-grained systems. First Edition. Beijing Sebastopol, CA: O’Reilly Media; 2015. 259 p. 

* [VER13] Vernon V. Implementing domain-driven design. Upper Saddle River, NJ; Mexico City: Addison-Wesley; 2013.

* [YOU79] Yourdon E, Constantine LL. Structured design: fundamentals of a discipline of computer program and systems design. Englewood Cliffs, N.J.: Yourdon Press; 1979.
