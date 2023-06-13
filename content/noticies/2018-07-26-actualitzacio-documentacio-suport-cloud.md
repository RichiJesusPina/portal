+++
date        = "2018-07-26"
title       = "Suport Cloud. Actualització documentació"
description = "Durant les darreres setmanes s'ha realitzat una tasca important d'actualització de documentació per part de l'equip de Suport Cloud, principalment d'elements de catàleg i tasques operatives a realitzar per part dels proveïdors d'aplicacions. En aquest article us fem un breu resum."
sections    = ["Notícies"]
categories  = ["cloud"]
key         = "AGOST2018"
+++

Durant les darreres setmanes s'ha realitzat una tasca important d'actualització de documentació per part de l'equip de Suport Cloud.

### Tasques operatives (Novetat!)

En el desplegament d'aplicacions al que anomenem Container Cloud, l'equip d'aplicacions ha d'assumir certes responsabilitats pròpies d'un model DevOps. En aquesta [plana](/cloud/2018-05-28-CLOUD-OPS-Cloud/) podeu trobar-ne el detall.

### Elements del catàleg

De forma periòdica es fa una actualització dels diferents elements que formen part del [catàleg](/cloud/cataleg/) de Suport Cloud. En el cas de les **imatges Docker** desenvolupades pel propi equip de Suport Cloud, l'objectiu és oferir sempre versions suportades de les diferents tecnologies i les més actuals possible. En cas que la tecnologia ofereixi una versió LTS (Long Term Support), com per exemple NodeJS, sempre s'intentarà estar alineat amb aquesta versió.

Com novetat important, s'ha repositat el [codi font](https://git.intranet.gencat.cat/3048-intern/imatges-docker) de les imatges Docker de Suport Cloud al [Git del SIC](https://git.intranet.gencat.cat/).
<br><br>
![suport-cloud-docker-images.png](/images/news/suport-cloud-docker-images.png)
<br>

Aquest codi font ha estat eliminat de Github, i les imatges Docker de DockerHub, passant al registre Docker corporatiu.

* https://github.com/gencat/ &rarr; https://git.intranet.gencat.cat/3048-intern/imatges-docker
* https://hub.docker.com/u/gencatcloud/ &rarr; https://registreimatges.sic.intranet.gencat.cat/

### Matriu catàleg Cloud

Per facilitar la recerca de disponibilitat d'elements de catàleg en un determinat Cloud Privat o Públic, es pot utilitzar la següent [matriu](https://qualitat.solucions.gencat.cat/estandards/estandard-full-ruta-programari/).

### Entorns de desenvolupament

Des de Suport Cloud es recomana utilitzar [entorns de desenvolupament](/cloud/entorns-dev/) el més alineats possible amb les [plataformes Container Cloud](/cloud/plataformes/). És per aquest motiu que quan s'actualitza un Container Cloud, s'actualitza també la versió de la plataforma de desenvolupament equivalent. És molt important sobretot a l'hora de definir els descriptors de desplegament (yaml), ja que canvis de versió poden suposar canvis en aquests descriptors.
