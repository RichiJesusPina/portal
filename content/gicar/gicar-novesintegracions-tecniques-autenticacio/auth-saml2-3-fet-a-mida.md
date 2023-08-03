+++
date        = "2018-08-17T12:45:42+01:00"
title       = "SAML2 - Aplicació feta a mida autenticant via SAML2 sense utilitzar Agent de Shibboleth (Canigó SAML o llibreria de tercers) "
description = "Aplicació feta a mida consumint servei d'autenticació SAML2 sense ús d'Apache-Shibboleth"
sections    = "gicar-novesintegracions-tecniques-autenticacio"
taxonomies  = []
toc			= false
weight 		= 3
+++

Com es deia en la introducció, GICAR disposa d’un “Identity Provider”, construït amb la tecnologia Shibboleth, amb capacitat de facilitar tiquets SAML2 a les aplicacions. 

La informació que l'aplicació requereix del proveïdor d'autenticació, en aquest cas GICAR, és el fitxer de metadades XML que defineix els parèmetres bàsics pels quals es regeix el IDP. Aquests fitxers de metadades es poden descarregar de:

	PREPRODUCCIÓ:
	https://preproduccio.autenticaciogicar4.extranet.gencat.cat/idp/shibboleth
	https://preproduccio.autenticaciogicar2.extranet.gencat.cat/idp2/shibboleth
	https://preproduccio.autenticaciogicar1.extranet.gencat.cat/idp1/shibboleth

	PRODUCCIÓ:
	https://autenticaciogicar4.extranet.gencat.cat/idp/shibboleth
	https://autenticaciogicar2.extranet.gencat.cat/idp2/shibboleth
	https://autenticaciogicar1.extranet.gencat.cat/idp1/shibboleth

Caldrà descarregar les metadades del endpoint que correspongui per a fer la integració.

En aquest punt en funció de la naturalesa amb la qual estigui feta l'aplicació, caldrà decidir com s'implementa la integració:

## Aplicació desenvolupada amb Framework Canigó:

**En cas que l'aplicació feta a mida estigui feta amb Canigó es recomana l'ús de la llibreria de Canigó per a la integració amb GICAR via SAML2.**
A través d'aquesta modalitat d'autenticació el framework Canigó és capaç d'integrar-se directament amb GICAR sense necessitat de cap llibreria externa. En aquest cas, veure documentació de Canigó [aquí](https://canigo.ctti.gencat.cat/howtos/2018-08-Canigo-SAML/).

## Aplicació NO desenvolupada amb Framework Canigó:

**En cas que l’aplicació no estigui feta amb Canigó, caldrà que el desenvolupador duguin a terme una de les següents dues opcions:**

***- OPCIÓ 1:*** 
Incorporar llibreries de tercers a l'aplicació per a generar i llegir la petició SAML2. Com a referència principal es pot cosultar la web https://www.samltool.com/, la qual disposa de nombrosos exemples i llibreries fetes en diverses tecnologies per a tractar les peticions SAML2.

***- OPCIO 2:*** 
Construir la petició SAML2 amb el codi javascript adjunt i decodificar-lo/desxifrar-lo posteriorment:

Per a dur a terme aquesta opció, cal construir un SAMLRequest que ha de contenir exactament la següent informació, estructurada de la següent forma:

	<?xml version="1.0"?> <samlp:AuthnRequest xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" Consent="urn:oasis:names:tc:SAML:2.0:consent:unspecified" Version="2.0" Destination="https://DOMINI _A_ESPECIFICAR_PER_L’EQUIP_GICAR/idp/profile/SAML2/Redirect/SSO" ID="xxxxxxxxxxxxxxxxxxxxx" IssueInstant="2014-12-24T10:35:25.4269359Z" IsPassive="false" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"> <saml:Issuer>https://URL del servei peticionari</saml:Issuer> <samlp:NameIDPolicy AllowCreate="True" Format="urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified" /> </samlp:AuthnRequest>

En cas que l’aplicació no pugui generar un SAMLRequest amb aquesta parametrització, no podrà integrar-se directament contra GICAR via SAML2, i haurà d’analitzar una altra via d’integració.

A continuació es proporciona un fragment de codi Javascript que, posat a dins d'una aplicació, permet redirigir a l'usuari cap a GICAR, generant automàticament un SAMLRequest.

	<script src="rawdeflate.js"></script>

	<script>

	//dades de l aplicacio a integrar
	var entityid="entity ID de l'aplicació integrada definida a GICAR";
	var AssertionConsumerServiceURL="URL de refirecció on espera el POST amb el SAMLResponse l'aplicació";

	//url endpoint de GICAR a utilitzar
	var endpointGICAR="https://A-DEFINIR-gicar.gencat.cat/idp/profile/SAML2/Redirect/SSO?SAMLRequest=";

	//calculem el id de la peticio
	var randomnumber = +new Date();
	//calculem la data d'emissio de la peticio. El desitjable és que la data de la petició es calculi en el servidor web i no en Javascript. Aquest fragment de codi per a calcular la data en Javascript és només per a fer proves, en entorn de producció la data s'hauria de calcular al servidor web de cara a assegurar que estigui generada per un rellotge sincronitzar amb un servidor NTP.
	var d = new Date();
	var curr_date = d.getDate();
	var curr_month = d.getUTCMonth()+1;
	var curr_month2 = (curr_month<10?'0':'') + curr_month;
	var curr_year = d.getFullYear();
	var ymd= curr_year+"-"+curr_month2+"-"+ curr_date+"T";
	var curr_hour = d.getUTCHours();
	var curr_min = d.getUTCMinutes();
	var curr_min2 = (curr_min<10?'0':'') + curr_min;
	var curr_sec = d.getUTCSeconds();
	var curr_msec = d.getUTCMilliseconds();
	var hms=curr_hour + ":" + curr_min2 + ":" + curr_sec + "." + curr_msec;
	var datasaml=ymd+hms;

	//generem samlrequest en pla
	var samlrequestpla="<?xml version=\"1.0\"?> <samlp:AuthnRequest xmlns:saml=\"urn:oasis:names:tc:SAML:2.0:assertion\" Version=\"2.0\" ID=\"b"+randomnumber+"a\" IssueInstant=\""+datasaml+"\" AssertionConsumerServiceURL=\""+AssertionConsumerServiceURL+"\" ProtocolBinding=\"urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST\" IsPassive=\"false\" xmlns:samlp=\"urn:oasis:names:tc:SAML:2.0:protocol\"> <saml:Issuer>"+entityid+"</saml:Issuer> </samlp:AuthnRequest>";

	//fem deflate i codificacio en base64
	var samlrequestb64 = btoa(RawDeflate.deflate(samlrequestpla));

	//codifiquem url
	var samlrequest=encodeURIComponent(samlrequestb64);


	var targeturl=endpointGICAR+samlrequest;

	//fem el redirect
	window.location.replace(targeturl);
	</script>

Aquest fragment de codi font té una dependència amb la llibreria "rawdeflate.js" que pot ser descarregada del següent [enllaç] (/related/gicar/rawdeflate.js)

Caldrà que l'equip desenvolupador faci una petició a l’equip GICAR per a incorporar aquesta aplicació com a consumidora del Identity Provider. Com a mínim els responsables de l'aplicació hauran de facilitar la següent informació a l’equip GICAR per a poder autenticar aquest servei:

- entityID = Haurà de contenir el valor del SAML:Issuer indicat en el SAMLRequest de les peticions que farà l’aplicació. Serà normalment la URL del servei peticionari.

- Certificat = Certificat X509 que farà servir l’aplicació consumidora per a desencriptar els tiquets SAML que haurà emès GICAR.

- AssertionConsumerService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" = S’haurà d’indicar a quina URL de l’aplicació GICAR haurà de redirigir un cop feta l’autenticació. Serà la URL on es farà la corresponent redirecció, i on es passarà per paràmetre l’atribut SAMLResponse amb el contingut de les respostes que haurà de decodificar i desencriptar l’aplicació.

Caldrà addicionalment que el desenvolupador especifiqui quina informació voldrà rebre referent a l’usauri que s’haurà autenticat.

- Caldrà finalment que el desenvolupador decodifiqui el paràmetre SAMLResponse que GICAR retornarà. Aquest paràmetre SAMLResponse contindrà tots els claims de resposta xifrats per defecte, i en casos excepcionals i per necessitats de l'aplicació serà possible que GICAR els envii en clar (assumint que el pas del paràmetre es farà per HTTPS). 


En les dues opcions caldrà que el desenvolupador recuperi els atributs de la resposta SAML" per a autoritzar l'usuari que s'ha autenticat. Els atributs prinipals són els següents:

- NIF de l'usuari (idem a la resposta GICAR_ID): "urn:oid:1.3.6.1.4.1.5923.1.1.1.6"
- Codi Intern de l'usuari (UID): "urn:oid:2.0.0.0.0.0.0.0.0.0.9"
- Mail de l'usuari: "urn:oid:0.9.2342.19200300.100.1.3"
- Resposta capçalera GICAR: "urn:oid:2.0.0.0.0.0.0.0.0.0.1"


