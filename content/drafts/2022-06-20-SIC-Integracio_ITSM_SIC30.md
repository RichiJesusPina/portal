+++
date        = "2022-06-20"
title       = "SIC. Integració ITSM al SIC 3.0"
description = "A partir del 26/09/2022 es posa en servei la integració amb ITSM Remedy pels desplegaments en modalitat automàtica o delegada als entorns de PRE i PRO al SIC 3.0."
#sections    = ["Notícies", "home"]
#categories  = ["SIC"]
#key         = "OCTUBRE2022"
+++

## Introducció

El **Servei d'Integració Contínua és un servei a disposició dels proveïdors d'aplicacions per a automatitzar el desplegament
de les aplicacions**. Per altra banda, **Remedy és l'eina corporativa per a la gestió de serveis de tecnologies de la informació (ITSM en anglès) i,
per tant, l'eina de gestió de desplegaments de les aplicacions**.

Fins al moment, el SIC disposava exclusivament d'una integració amb ITSM per a facilitar la petició de desplegaments a Cpd en modalitat semiautomàtica, és a dir,
generant un tiquet Remedy CRQ en mode "Draft" que el proveïdor ha d'acabar de complimentar amb la informació i tasques necessàries per a la petició de desplegament per part de Cpd.
Per la qual cosa, els desplegaments automàtics realitzats des del SIC, no disposaven de cap mena de traçabilitat a l'eina.

## Novetats

**Amb l'objectiu de disposar d'una traçabilitat completa dels desplegaments als entorns de Preproducció i Producció de les aplicacions,
a partir del 26/09/2022 es posa en servei la integració amb l'eina ITSM pels desplegaments en modalitat automàtica** (automàtica al cloud o delegada on-premise)
a aquests entorns. D'aquesta forma, el SIC passarà a proporcionar dues modalitats d'integració amb ITSM pels entorns de Preproducció i Producció:

- **(NOU) Automàtica**: en el cas de modalitat de desplegament automàtic al cloud o delegat, i amb la informació proporcionada per l'usuari,
el sistema s'encarrega de generar, actualitzar i tancar automàticament els tiquets Remedy CRQ associats a cada desplegament permetent
la traçabilitat dels desplegaments sense que es requereixi cap intervenció manual per part de l'usuari.

- Mode **Draft**: en cas de modalitat de desplegament semiautomàtica, el sistema no pateix canvis i seguirà encarregant-se de generar
una plantilla de petició de canvi que el proveïdor ha d'acabar de complimentar per a poder sol·licitar a Cpd el corresponent desplegament.

## Requisits

**Els usuaris amb rol Release Manager al SIC que aproven els desplegaments sobre aquests entorns han de tenir el rol de
coordinador de canvis a Remedy**. En cas contrari, el sistema assignarà com a coordinador un altre usuari dins del grup.
Per a sol·licitar la seva revisió, cal fer una petició a l'equip de Remedy.

## Funcionament

La **nova modalitat d'integració automàtica aplica a la següent tipologia de pipelines de desplegament de versions del SIC 3.0: `DEPLOY`, `DEPLOY-TAG`,
`DEPLOY-ALL`, `DEPLOY-DESCRIPTORS`, `DELETE-DESCRIPTORS` i `DEPLOY-APIM`**.

El funcionament previst és que, en el moment en què s'inicia el desplegament de l'entorn
en l'**etapa "Deploy confirmation"**, l'usuari haurà d'indicar la informació necessària per a la generació del tiquet Remedy de canvi (CRQ):
servei associat, categorització i informació del desplegament. Per exemple, per al desplegament a l'entorn de Producció, es mostrarà
un formulari com el següent:

![Input request](/related/sic/3.0/pipeline-input-request-itsm.png)

<br/>
Un cop indicada la informació requerida i confirmat el desplegament, a l'**etapa "ITSM Register"** el sistema generarà automàticament
el tiquet Remedy CRQ amb tota la informació necessària, assumint, com a dates de planificació del canvi, la data i hora en què
s'inicia el desplegament a l'entorn i una previsió estàndard de finalització de 30 minuts.

Tan bon punt finalitzi el desplegament a l'entorn pertinent, incloent-hi possibles tasques pre-post desplegament, a l'**etapa "ITSM Close"**
el sistema transicionarà el tiquet a l'estat "Implementation in Progress" indicant la data i hora d'inici real del desplegament i,
a continuació, es tancarà el tiquet d'acord amb el resultat obtingut:

- Si el **desplegament finalitza correctament**: el tiquet transiciona a Estat = Closed – Successful enregistrant la data i hora de finalització
del desplegament. En aquest cas la pipeline no s'atura i continua amb les següents etapes.

- Si el **desplegament finalitza amb errors**: el tiquet transiciona a Estat = Closed – Unsuccessful enregistrant la data i hora de finalització
del desplegament. En aquest cas la pipeline es cancel·la indicant que s'han produït errors en el desplegament.

<br/>
Per a més informació:

- [Servei d'Integració Contínua](/sic30-serveis/ci/)

<br/><br/>
Si teniu qualsevol dubte o problema podeu revisar les [**Preguntes Freqüents**] (/sic/faq) o utilitzar els canals de [**Suport**] (/sic/suport).