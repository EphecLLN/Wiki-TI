---
layout: default
title: VisioConference
parent: Réseaux
---

# Les Protocoles de visioconférence

## 1. Que signifie visioconférence  [[1]](https://fr.wikipedia.org/wiki/Visioconférence)

La visioconférence est une solution de communication à distance sécurisée, via Internet, dans des points multiples, permettant l'échange visuel entre deux individus, ou plusieurs dizaines ou même des centaines ou des milliers de personnes, selon les options et les licences.
Il est aussi appelé vidéoconférence, c'est la technique qui permer de voir et dialoguer avec son interlocuteur à travers un moyen numérique.

Visioconférence est aussi la combinaison de 2 techniques :

- <b>La visiophonie ou vidéotéléphonie :</b> qui est l'association de la téléphonie et de la télévision permettant aux utilisateurs de se voir pendant leur conversation téléphonique.

- <b>La conférence multipoints ou conférence à plusieurs :</b> permettant d'effectuer une réunion avec plus de 2 terminaux

Figure 1. Ex: visioconférence
<img src="http://image.noelshack.com/fichiers/2022/31/7/1659872888-visioconference-au-bureau.jpg" width="800" height="350"> 

## 2. Pourquoi la visioconférence ? [3]( https://www.a2com.fr/blog/6-bonnes-raisons-dutiliser-la-visioconference-en-entreprise/#:~:text=Le%20collaboratif%20est%20au%20cœur,dans%20une%20entreprise%20multi%20sites.)
La visioconférence est un vrai levier d’efficacité professionnel qui facilite les échanges entre les collaborateurs dans les entreprise multi sites.

### 2.1 Les principaux prorocoles de visioConférence
 
##### Figure 2. certains protocoles [[2]](https://trueconf.com/video-conferencing-architecture.html)
<img src="http://image.noelshack.com/fichiers/2022/33/6/1660971322-princproo.png" width="800" height="350"> 

#### Protocole H.323
 C'est un protocole de transfert de données avec une bande passante non garantie appliquée aux vidéoconférences personnelles et de groupe.
 - Il a été conçu pa r l'UIT(Union internationale des télécommunication)
 - Il utilise le langage informatique du binaire
 - Assure des communications multimédias sur différents réseaux.
 - il est très complexe, il est possible qu'il y ait des retards.
<img src="http://image.noelshack.com/fichiers/2022/31/7/1659878808-h323.png" width="800" height="350">

#### Protocole SIP
C'est un protocole réseau permettant de connecter des applications clientes de différents fournisseurs. SIP a remplacé H.323 et est utilisé dans la visioconférence et la téléphonie IP.
- Il a été conçu par l'IETF(Internet Engineering Task Force)
- Il utilise le langage informatique ASCII
- Il est très flexible et simple 
- L'agent utilisateur et le serveur réseau constituent deux composantes fonctionnelles principales.
- Permet de configurer, d'éditer et de mettre fin aux sessions multimédia ou aux appels.
- Ressemble au protocole HTTP.
- Il est fonctionnel avec le IPv4 et IPv6
- 
### 2.2 Différentes normes pour la visioconférence [4]( https://www.dwpro.fr/blog/visioconference/normes-visioconference#:~:text=De%20plus%2C%20la%20norme%20H,les%20systèmes%20de%20visioconférence%204K.)

#### Contrôle et la signalisation :
H.225 : il est en en système de communications, est une sous-norme de H.323 [I]( https://fr.wikipedia.org/wiki/H.225).
Ses objectifs :
- Gestion d’un appel : Ce système s'appuie sur le processus de préparation des appels ISDN (Intergrated Services Digital Network)
- Enregistrement, admission et status : RAS qui est le premier canal de signalisation et utilise le UDP comme couche de transport 
H.245 : il a été mise en place pour les canaux des médias par H.323, il est ouvert dès le départ à la négociation de codecs communs, veiller à toutes les fonctions de gestion des flux de médias.
RTCP (Real-time Transport Control Protocol) : Il s'agit d'un protocole de contrôle des flux RTP, permettant de transmettre des informations de base sur les participants d'une session, ainsi que sur la qualité de service.[II]( https://fr.wikipedia.org/wiki/Real-time_Transport_Control_Protocol )
#### Voix : 
- G.726 : 
- G.728 : 
- G.729 : 
#### Video :
- H.261 : 
- H.263 :
- H.265 : H265 ou encore HEVC (High Efficiency Video Coding) un nouveau standard dont l'objectif est d'améliorer de manière significative la compression vidéo, il est en mesure de transmettre la même vidéo que son prédécesseur (H264) pour une vitesse deux fois moins. Il supporte toutes les définitions d’images usuelles et des cadences d’images plus élevées.
#### Données :
- T.123 : 
- T.124 : 
- T.125 :

## 3 Difference entre la visioConférence et le streaming video 
### 3.1 VisioConférence
### 3.2 Streaming Video
est un type de technologie multimédia qui offre du contenu vidéo et audio à un dispositif connecté à Internet. C’est un flux vidéo organiser par l’organisateur comme video sur youtube, Netflix etc.<bt>
#### 3.2.1 Types de streaming video
 - Streaming en direct 
 - streaming video
#### 3.2.2 Les protocoles de streaming vidéo:
-	HLS
-	SRT
-	MPEG
-	WebRTC
## Conclusion

## Bibliographie:
1. https://trueconf.com/video-conferencing-architecture.html
2. https://fr.wikipedia.org/wiki/Visioconférence
3. https://www.zdnet.fr/lexique-it/visioconference-une-definition-39928673.htm#:~:text=La%20visioconférence%2C%20ou%20vidéoconférence%20(synonyme,selon%20les%20options%20et%20licences.
4. https://fr.acervolima.com/protocoles-de-visioconference/
5. https://www.cisco.com/c/fr_ca/support/docs/quality-of-service-qos/qos-packet-marking/21662-video-qos.html
6. https://docplayer.fr/508783-Deploiement-de-la-visioconference-ip-dans-un-etablissement-etat-de-l-art-et-evolution-des-protocoles.html
