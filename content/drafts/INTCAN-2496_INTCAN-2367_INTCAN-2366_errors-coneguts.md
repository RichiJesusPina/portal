+++
date        = "2022-05-30"
title       = "Errors coneguts"
description = "Errors coneguts màquina virtual 3.1.0"
sections    = "canigo-fwk-docs"
weight		= 2
+++

## Errors coneguts versió 3.1.0

* Compartir carpetes des del Host. Per a que funcioni s'han d'instal·lar les VirtualBox guest additions,
seguint les instruccions del howto [Com instal·lar les VBOX guest additions]
(/howtos/2022-05-30-Howto-Instalar-guest-additions-entorn-desenvolupament-canigo/).

* El sistema d'àudio (hda, ac97) no funciona correctament degut a un error relacionat amb la versió de
VirtualBox (p.e. 5.2.18). En versions més noves (>= 6.0.0) pot estar resolt (tot i que no s'ha verificat aquest punt).
Podeu trobar més informació al següent enllaç: [https://forums.virtualbox.org/viewtopic.php?f=8&t=91190]
(https://forums.virtualbox.org/viewtopic.php?f=8&t=91190).