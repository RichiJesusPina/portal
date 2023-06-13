+++
date        = "2022-02-21"
title       = "SIC 2.0 - Utilitzar imatges Docker Builder"
description = "SIC 2.0 -Howto per mostrar com utilitzar les imatges Docker del catàleg d'imatges de construcció del SIC"
section     = "howtos"
categories  = ["SIC"]
#key        = "JUNY2019"
+++

## Introducció

El SIC actualment utilitza la [tecnologia Docker](https://www.docker.com/) per a disposar d’un **entorn aïllat i immutable
de construcció que, a més pugui ser utilitzat i testejat pels propis proveïdors**. Aquest how-to va dirigit a tots aquells
perfils tècnics que tinguin la necessitat de simular i executar les imatges Docker en un entorn local tal i com ho realitza el SIC.

## Harbor

Docker per defecte està configurat per a utilitzar el registre públic [Docker Hub](https://hub.docker.com/) com a repositori d’imatges.
No obstant, **les imatges que utilitzarà SIC per a la construcció es troben allotjades un registre docker privat**
escollit per la Generalitat de Catalunya: [Harbor](https://goharbor.io/).

![Pipeline del SIC](/related/sic/3.0/harbor_docker_images.png)
</br>

Podeu accedir al codi font del catàleg d'imatges del SIC, i a la documentació associada, mitjançant el següent enllaç: </br>
https://git.intranet.gencat.cat/0192-intern/docker-images.

![Pipeline del SIC](/related/sic/3.0/docker_images_project.png)
</br>

## Ús del registre privat

El registre Docker privat de la Generalitat de Catalunya, està disponible a: https://registreimatges.sic.intranet.gencat.cat.
Es tracta d’un registre privat sense cap repositori d'accés públic.

### Permisos d'accés
Per a disposar d'accés a les imatges Docker utilitzades al SIC és necessari contactar amb l'Oficina Tècnica de Canigó a través dels
canals establerts: https://canigo.ctti.gencat.cat/sic/suport/. L'Oficina subministrarà al proveïdor d’aplicacions un usuari
amb permís de lectura al projecte **gencatsic** que conté les imatges Docker utilitzades pel SIC.

### Accés via consola web
Es pot accedir al Harbor a través de la seva consola web mitjançant: https://registreimatges.sic.intranet.gencat.cat.
Un cop dins es pot navegar a través de les carpetes de les imatges del projecte **gencatsic**.

### Login
Per a poder descarregar les imatges en local primer ens hem de autenticar de la següent manera:
```
docker login https://registreimatges.sic.intranet.gencat.cat
```

Si no es tracta d'una imatge d'un projecte públic, caldrà introduir les credencials Gicar de l'usuari.

### Descàrrega d'imatges

Un cop realitzada l’autenticació per linea de comandes, podem baixar-nos la imatge escollida mitjançant:
```
docker pull registreimatges.sic.intranet.gencat.cat/gencatsic/maven-builder:1.0-3.6-8
```

On, en el cas d'exemple, estem descarregant-nos la imatge *maven-builder* versió 1.0-3.6-8.

### Execució de les imatges

Un cop descarregada la imatge Docker, la podem executar en el nostre entorn local mitjançant:
```
docker run -it --rm \
 --name gencatsic-maven-builder \
 -v $HOME/.m2/repository:/repository \
 -v $HOME/.m2/settings.xml:/settings/settings.xml \
 -v $PWD:/app -w "/app" \
 registreimatges.sic.intranet.gencat.cat/gencatsic/maven-builder:1.0-3.6-8 \
 mvn clean install -Dmaven.test.skip=true
```

En aquest cas estem indicant que volem:

- Que el nom de la imatge executant-se tingui el nom *gencatsic-maven-builder*.

- Compartir el nostre repository maven, ubicat a *$HOME/.m2/repository*, amb el repository maven de la imatge Docker .

- Compartir el nostre settings.xml, ubicat a $HOME/.m2/settings.xml, amb el settings.xml de la imatge Docker.

- Compartir el codi font de la nostre aplicació, ubicat a *$PWD*, amb el directori de treball de la imatge Docker.

- Executar el goal de maven *mvn clean install -Dmaven.test.skip=true*.

**IMPORTANT**: en cas de fer ús d'una imatge amb Jdk 1.7 o inferior, caldrà forçar el protocol TLS 1.2 mitjançant el següent paràmetre: `-Dhttps.protocols=TLSv1.2`.
En cas contrari, es podria rebre errors del tipus *Received fatal alert: protocol_version* si es fa ús del repositori d'artefactes públics del SIC.

### Logout

Si volem desconnectar-nos del Harbor serà necessari realitzar un logout mitjançant:
```
docker logout https://registreimatges.sic.intranet.gencat.cat
```

## Estendre d’imatges Docker de SIC

És possible generar una imatge Docker heretant d'una imatge del catàleg de SIC.
Per a fer-ho, s'ha d’incloure al fitxer `Dockerfile` la instrucció [FROM](https://docs.docker.com/engine/reference/builder/#from)
seguit del nom de la imatge base a utilitzar.
Per exemple:

```bash
FROM registreimatges.sic.intranet.gencat.cat/gencatsic/maven-builder:1.0-2.2-8
```
</br>


Per tal d’evitar errors en la construcció de la imatge estesa, cal tenir en compte algunes recomanacions:

* Els usuaris s'hereten de la imatge base i, per defecte, els usuaris de les imatges del SIC disposen de permisos limitats i
destinats exclusivament a la construcció d'artefactes. És a dir, si es necessita instal·lar o executar algun programa addicional serà
necessari invocar la instrucció [USER](https://docs.docker.com/engine/reference/builder/#user) per a canviar l'usuari a *root*.

* Si es vol mantenir el comportament predeterminat de les imatges del SIC serà necessari agregar al final
la instrucció [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#entrypoint) amb la mateixa instrucció que està
configurada a la imatge base i assegurar-se que l'usuari d'execució del contenidor es correspon amb el que s'utilitza a la imatge base.

* Cal revisar el fitxer `Dockerfile` de la imatge base del SIC a utilitzar per a assegurar-se que les instruccions que conté
apliquen per a la nova imatge. Per exemple, en una imatge estesa l'equip responsable del manteniment i suport és diferent, per lo que cal
canviar el [MAINTAINER](https://docs.docker.com/engine/reference/builder/#maintainer-deprecated) i/o
[LABEL](https://docs.docker.com/engine/reference/builder/#label) per a indicar l’adreça de correu adient.
</br>

Exemple:

```bash
# S'utilitza una imatge base del SIC.
FROM registreimatges.sic.intranet.gencat.cat/gencatsic/maven-builder:1.0-2.2-8

# Es modifica el responsable de la imatge.
LABEL maintainer="change.me@gencat.cat"

# Es modifica l'usuari a root per a crear una variable d'entorn, instal·lar un programa addicional, donar permisos i
eliminar fitxers innecessaris.
USER root
ENV FLEX_HOME='/flex-sdk'

RUN apk --update add --no-cache --quiet --virtual .build-deps curl unzip \
&& curl -fsSL -o /tmp/flex-sdk.zip http://download.macromedia.com/pub/flex/sdk/builds/flex3/flex_sdk_3.4.1.10084A.zip \
&& curl -fsSL -o /tmp/flex-sdk-libs.zip http://download.macromedia.com/pub/flex/sdk/datavisualization_sdk3.4.zip \
&& unzip /tmp/flex-sdk.zip -d "${FLEX_HOME}" \
&& unzip /tmp/flex-sdk-libs.zip -d "${FLEX_HOME}" \
&& chown -R maven:maven ${FLEX_HOME} \
&& chmod -R a+rx ${FLEX_HOME} \
&& apk del .build-deps \
&& rm -rf /tmp/*

# S'assegura que l'usuari d'execució dels contenidors associats a la imatge es correspongui amb l'utilitzat a la imatge
base i que, si la imatge base té un ENTRYPOINT, aquest sigui invocat.
USER maven
ENTRYPOINT ["/usr/local/bin/mvn-entrypoint.sh"]
```