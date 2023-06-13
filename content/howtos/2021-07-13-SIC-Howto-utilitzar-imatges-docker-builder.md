+++
date        = "2021-07-13"
title       = "SIC 3.0 - Utilitzar imatges Docker Builder"
description = "SIC 3.0 - Howto per mostrar com utilitzar les imatges Docker del catàleg d'imatges de construcció"
#section     = "howtos"
#categories  = ["SIC"]
#key        = "SETEMBRE2021"
+++

## Introducció

El SIC actualment utilitza la [tecnologia Docker](https://www.docker.com/) per a disposar d’un **entorn aïllat i immutable
de construcció que, a més pugui ser utilitzat i testejat pels mateixos proveïdors**. Aquest how-to va dirigit a tots aquells
perfils tècnics que tinguin la necessitat de simular i executar les imatges Docker en un entorn local tal i com ho realitza el SIC.

## Ús del registre privat

En el [Registre d'imatges corporatiu](/sic30-serveis/registre-imatges/) podreu trobar informació sobre el registre
d'imatges, el codi font, la documentació associada i el procediment per a disposar d'accés.

### Accés via consola web

Es pot accedir al Harbor a través de la seva consola web mitjançant: <https://registreimatges.sic.intranet.gencat.cat>.
Un cop dins es pot navegar a través de les carpetes de les imatges del projecte
[**gencat-sic-builders**](https://registreimatges.sic.intranet.gencat.cat/harbor/projects/129/repositories).

### Login

Per a poder descarregar les imatges en local primer ens hem d'autenticar de la següent manera:

```bash
docker login https://registreimatges.sic.intranet.gencat.cat
```

Si no es tracta d'una imatge d'un projecte públic, caldrà introduir les credencials Gicar de l'usuari.

### Descàrrega d'imatges

Un cop realitzada l’autenticació per línia de comandes, podem baixar-nos la imatge escollida mitjançant:

```bash
docker pull registreimatges.sic.intranet.gencat.cat/gencat-sic-builders/mvn-builder:1.0-3.6-8
```

On, en el cas d'exemple, estem descarregant-nos la imatge *mvn-builder* versió 1.0-3.6-8.

### Execució de les imatges

Un cop descarregada la imatge Docker, la podem executar en el nostre entorn local mitjançant:

```bash
docker run -it --rm \
  --user="1000:$(id -g)" \
  --net=host \
  --name gencat-sic-builder \
  -v $HOME/.m2/settings.xml:/mnt/nexus/settings.xml \
  -v $HOME/.npmrc:/mnt/nexus/.npmrc:z \
  -v $PWD:/app \
  -w "/app" \
  registreimatges.sic.intranet.gencat.cat/gencat-sic-builders/mvn-builder:1.0-3.6-8 \
  mvn clean package
```

En aquest cas estem indicant què volem:

- Que s'utilitzi l'usuari `1000` (necessari per a totes les imatges builders al SIC 3.0) i el grup de l'usuari *Host*.

- Que s'utilitzi el controlador `Host` de docker per a compartir la xarxa del host.

- Que el nom de la imatge executant-se tingui el nom `gencat-sic-builder`.

- Que s'injectin com a volums els fitxers de configuració de maven i npm si són necessaris a la imatge.

- Executar el goal de maven `mvn clean package`.

### Logout

Si volem desconnectar-nos del Harbor, serà necessari realitzar un logout mitjançant:

```bash
docker logout https://registreimatges.sic.intranet.gencat.cat
```

## Estendre d’imatges Docker de SIC

És possible generar una imatge Docker heretant d'una imatge del catàleg de SIC.
Per a fer-ho, s'ha d’incloure al fitxer `Dockerfile` la instrucció [FROM](https://docs.docker.com/engine/reference/builder/#from)
seguit del nom de la imatge base a utilitzar.
Per exemple:

```bash
FROM registreimatges.sic.intranet.gencat.cat/gencat-sic-builders/mvn-builder:1.0-3.6-8
```

</br>

Per tal d’evitar errors en la construcció de la imatge estesa, cal tenir en compte algunes recomanacions:

- Els usuaris s'hereten de la imatge base i, per defecte, els usuaris de les imatges del SIC disposen de permisos limitats i
destinats exclusivament a la construcció d'artefactes. És a dir, si es necessita instal·lar o executar algun programa addicional serà
necessari invocar la instrucció [USER](https://docs.docker.com/engine/reference/builder/#user) per a canviar l'usuari a *root*.

- Si es vol mantenir el comportament predeterminat de les imatges del SIC, serà necessari agregar al final
la instrucció [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#entrypoint) amb la mateixa instrucció que està
configurada a la imatge base i assegurar-se que l'usuari d'execució del contenidor es correspon amb el que s'utilitza a la imatge base.

- Cal revisar el fitxer `Dockerfile` de la imatge base del SIC a utilitzar per a assegurar-se que les instruccions que conté
apliquen per a la nova imatge. Per exemple, en una imatge estesa l'equip responsable del manteniment i suport és diferent, per tant, cal
canviar el [MAINTAINER](https://docs.docker.com/engine/reference/builder/#maintainer-deprecated) i/o
[LABEL](https://docs.docker.com/engine/reference/builder/#label) per a indicar l’adreça de correu adient.

- Altres criteris i recomanacions: [Criteris creació contenidors docker](/cloud-caas/dockerImages).

</br>

Exemple:

```bash
# S'utilitza una imatge base del SIC.
FROM registreimatges.sic.intranet.gencat.cat/gencat-sic-builders/mvn-builder:1.0-2.2-8

# Es modifica el responsable de la imatge.
LABEL maintainer="change.me@gencat.cat"

# Es modifica l'usuari a root per a crear una variable d'entorn, instal·lar un programa addicional, donar permisos i eliminar fitxers innecessaris.
USER root
ENV FLEX_HOME='/flex-sdk'
RUN apk add --no-cache --virtual .build-deps curl tar unzip procps \
  && curl -fsSL -o /tmp/flex-sdk.zip http://download.macromedia.com/pub/flex/sdk/builds/flex3/flex_sdk_3.4.1.10084A.zip \
  && curl -fsSL -o /tmp/flex-sdk-libs.zip http://download.macromedia.com/pub/flex/sdk/datavisualization_sdk3.4.zip \
  && unzip /tmp/flex-sdk.zip -d "${FLEX_HOME}" \
  && unzip /tmp/flex-sdk-libs.zip -d "${FLEX_HOME}" \
  && chown -R 1000:1000 ${FLEX_HOME} \
  && chmod -R a+rx ${FLEX_HOME} \
  && apk del .build-deps \
  && rm -rf /tmp/* \
  && java -version \
  && mvn --version

# S'assegura que l'usuari d'execució dels contenidors associats a la imatge es correspongui amb l'utilitzat a la imatge base
USER 1000
CMD ["mvn", "-version"]
```