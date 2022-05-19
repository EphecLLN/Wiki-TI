---
layout: default
title: INTEGRATION DE FLUX VIDEO DANS UNE INFRASTRUCTURE ASTERIX
parent: Réseaux
---

[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)

# ASTERIX

## Qu'est-ce que ASTERIX ?

Astérix est une implémentation logicielle d'un autocommutateur privé (PBX) qui
est une centrale téléphonique desservant une organisations privée et permettant
le partage des lignes réseaux d'un bureau central entre les téléphones installés
en interne.

Chaque Appareil connecté au PBX possède un numéro de téléphone d'extension
désigné qui peut ou non être mappé au plan de numérotation du bureau central.

Astérisk permet donc le l'etablissement et le contrôle de la communication entre
les différents terminaux tels que les postes téléphonique, les appareils ou
services sur voix sur protocole internet (VoIP).

Il prend en charge plusieurs fonctionnalités disponible dans les systèmes PBX
notament la messagerie vocale, conférence téléphonique, distribution
automatiqued'appel. Ce qui fait l'une des plus grande force d'Asterisk est que
les utilisateurs peuvent créer de nouvelle fonctionnalités en écrivant des
scripts de plan de numérotation dans plusieurs des propres langages d'
extensions d'Asterisk , en ajoutant des modules chargeables personnalisés écrits
en PHP ou C. Mais qu'en est-il de l'intégration des flux video dans une
infrastructure Asterisk ?

## Flux video

Encore appelé streaming, c'est un processus d'envoi de contenu en direct. La
plupart des logiciels de voix sur ip telles que facebook, skype discord intègre
des flux multimédia : appel video. En générale le VoIp est utilisé pour décrire
des communications point à point. Pour ce qui est du flux video par
visioconférence on parle de communication multipoint qui est une diffusion d'un
emetteur vers un groupe de récepteur.

Pour ce faire, les deux protocoles utilisés dans une infrastructure Astérisk
pour permettre ce flux video sont RTP et RTCP

### Protocole RTP

Le principe du protocole RTP (Real-Time Transfert Protocol) consiste à envoyer
les paquets en temps réel sur le réseau. Les paquets sont marqués temporellement
de manière à être réordonnancés par le client afin d’afficher la vidéo de
manière cohérente. Il permet de founir un moyen uniforme pour le transport de
données sur IP soumises à des contraintes de temps réel tels que les flux média,
audio et vidéo.

- Caractéristiques

RTP étant utilisé en mode unidirectionnel (d'un emetteur à un récepteur), il
peut être aussi utilisé en mode multicast via satellite.

Le rôle principale de RTP consiste à numéroter les paquets IP afin de
reconstituer les flux voix ou vidéo de manière fluide et ce, même si le réseau
sous jacent change l'ordre des paquets

- Utilisation en mode multicast

La mise en œuvre de RTP en mode multicast requiert la configuration préalable de
routage au niveau du récepteur, qui doit faire lui-même la demande de routage à
ses routeurs hôtes, entre l'émetteur et le récepteur. L'émetteur quant à lui
informe séparément les routeurs de diffusion auxquels il est directement
connecté.

### Protocole RTCP

C'est un protocole de contrôle des flux RTP, permettant de récupérer des
informations sur les participants d'une session, et sur la qualité de service en
transmettent périodiquement des paquets de contrôles.
