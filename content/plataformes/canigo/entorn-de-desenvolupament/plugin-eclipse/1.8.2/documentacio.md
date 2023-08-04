+++
date        = "2021-12-17"
title       = "Documentació"
description = "Documentació plugin eclipse 1.8.2"
sections    = "canigo-fwk-docs"
weight		= 3
+++

## Introducció

### Propòsit

El plugin de Canigó per a Eclipse permet crear de forma automàtica l'esquelet d'una aplicació Canigó. L'aplicació que es crea està orientada a servir com a punt de partida per a la creació d'una aplicació més complex, però des del moment de la seva creació ja és desplegable i funcional.

*NOTA*: Tot i que les passes d'instal·lació i configuració del plugin són senzilles, [l'entorn de desenvolupament de Canigó](/canigo-fwk-docs/entorn-de-desenvolupament/maquina-virtual/) conté un Eclipse amb aquest plugin configurat i llest per ser emprat.

#### Eclipse 2019-03

L'última versió del plugin està certificada amb la versió d'Eclipse 2019-03, que podeu trobar al següent enllaç: https://www.eclipse.org/downloads/packages/release/2019-03/r/eclipse-ide-enterprise-java-developers

### Prerequisit

Abans de realitzar la instal·lació del plugin Canigó s'ha de configurar el settings.xml del Maven per a tenir referenciats el [repositori de Canigó] (/canigo-fwk-docs/documentacio-llibreries/)

### Instal·lació

A la pestanya Help de l'Eclipse seleccionar "Install New Software..."

![](/related/canigo/documentacio/plugin-canigo/img1.jpg)

Prémer sobre el botó Add i afegir el respository de Canigó:
http://repos.canigo.ctti.gencat.cat/repository/maven2/cat/gencat/ctti/canigo.plugin/update-site/

![](/related/canigo/documentacio/plugin-canigo/img2.jpg)

Seleccionar **Plug-in 1.8.2 - Canigo 3.6.2**

![](/related/canigo/documentacio/plugin-canigo/Plugin_eclipse_1_7_10.png)

## Crear Aplicació Canigó

A la vista Package Explorer de l'Eclipse fer botó dret: New -> Other

![](/related/canigo/documentacio/plugin-canigo/img4.jpg)

Seleccionar Assistent Projectes Canigó -> Crear un Projecte Canigó

![](/related/canigo/documentacio/plugin-canigo/Plugin_eclipse_1_7_4_new_project.png)

El plugin genera una aplicació REST amb un CRUD de demo .

Es dóna llibertat per triar la tecnologia per a realitzar el front-end (AngularJS, Bootstrap, EmberJS...) sempre que es compleixi el PIV de Gencat.

L'aplicació inicial que es crea amb el plugin inclou d'inici:

* mòdul de persistència (Base de dades a memòria H2)
* mòdul d'administració de logs

Per a desplegar l'aplicació només s'ha de compilar "mvn install" i arrencar l'aplicació utilitzant Spring Boot.

![](/related/canigo/documentacio/plugin-canigo/img11.jpg)

L'aplicació demo porta incorporat Swagger i és aquesta la pantalla que es carrega quan s'accedeix a http://localhost:8080/canigo-api.html

Per defecte, les APIs REST es publiquen a http://localhost:8080/api/*:

Exemples:

* http://localhost:8080/api/equipaments/
* http://localhost:8080/api/equipaments/1

La url per accedir al mòdul d'administració de logs és http://localhost:8080/loggingAdministration.html

Per a més informació sobre aquest mòdul consultar la [documentació](/canigo-fwk-docs/documentacio-per-versions/3.6LTS/3.6.2/moduls/moduls-generals/modul-logging-admin/)

## Afegir/Esborrar nous mòduls

Per afegir o treure mòduls de Canigó el plugin proporciona la possibilitat de fer-ho de forma automàtica.

S'ha de prémer sobre el projecte, botó dret -> Canigó

![](/related/canigo/documentacio/plugin-canigo/img9.jpg)

Per exemple, per a afegir el mòdul de seguretat el plugins ens donarà l'opció a triar si es desitja utilitzar JWT, el provider de seguretat a utilitzar (Arxiu, BBDD o Gicar) i si es desitja utilitzar SAML

![](/related/canigo/documentacio/plugin-canigo/Plugin_eclipse_1_7_4_add_modules_security.png)

