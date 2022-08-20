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

<img src="http://image.noelshack.com/fichiers/2022/33/6/1660987249-couchepro.png" width="800" height="350">


#### Contrôle et la signalisation :
H.225 : il est en système de communications, est une sous-norme de H.323 [I]( https://fr.wikipedia.org/wiki/H.225).<br>
Ses objectifs :
- Gestion d’un appel : Ce système s'appuie sur le processus de préparation des appels ISDN (Intergrated Services Digital Network)
- Enregistrement, admission et status : RAS qui est le premier canal de signalisation et utilise le UDP comme couche de transport 
H.245 : il a été mise en place pour les canaux des médias par H.323, il est ouvert dès le départ à la négociation de codecs communs, veiller à toutes les fonctions de gestion des flux de médias.
RTCP (Real-time Transport Control Protocol) : Il s'agit d'un protocole de contrôle des flux RTP, permettant de transmettre des informations de base sur les participants d'une session, ainsi que sur la qualité de service.[II]( https://fr.wikipedia.org/wiki/Real-time_Transport_Control_Protocol )
#### Voix : 
- G.726 : C'est une norme de compression audio de l'UIT-T, il est utilisée en telephonie fixe, Il utilise une "modulation impulsive et l'encodage différentiel adaptatif" (MICDA) à 40, 32, 24 ou 16 kbit/s
- G.728 : Ancienne norme decompression audio, Recommandation relative aux protocoles H.320 et H.323 sur l'encodage sonore pour la téléphonie et la vidéoconférence.
- G.729 : Norme qui est sous recommandation, définit l'encodage vocal 8 kbit/s par prédiction linéaire avec excitation par séquences encodées avec structure algébrique conjuguée.
#### Video :
- H.261 : C'est une évolution de la norme,Elle fut notamment popularisée par la console de jeu PlayStation de Sony, qui l'intégra entièrement dans son moteur de décompression de données
- H.263 : Ce standard est devenu obsolète, sa succession a été assurée par le standard H.263 de meilleure qualité pour VoIP ; pour le codage vidéo pur, nous préférons le standard H.264.
- H.265 : H265 ou encore HEVC (High Efficiency Video Coding) un nouveau standard dont l'objectif est d'améliorer de manière significative la compression vidéo, il est en mesure de transmettre la même vidéo que son prédécesseur (H264) pour une vitesse deux fois moins. Il supporte toutes les définitions d’images usuelles et des cadences d’images plus élevées.
#### Données :
- T.123 : Audiovisual Protocol Stacks
- T.124 : Generic Conference Control
- T.125 : Multipoint Communication Service

## 3 Difference entre la visioConférence et le streaming video 
### 3.1 VisioConférence
est mieux défini comme un événement où plusieurs utilisateurs d'ordinateurs peuvent communiquer entre eux simultanément à l'aide de caméras sur Internet.
### 3.2 Streaming Video
est un type de technologie multimédia qui offre du contenu vidéo et audio à un dispositif connecté à Internet. C’est un flux vidéo organiser par l’organisateur comme video sur youtube, Netflix etc.<bt>
#### 3.2.1 Types de streaming video
 - Streaming en direct 
 - streaming video
#### 3.2.2 Les protocoles de streaming vidéo:
-	HLS (HTTP Live Streaming): Ce protocole est compatible avec un large éventail d'appareils, navigateurs de bureau, téléviseurs intelligents, boîtes de décodage, appareils mobiles Android et iOS et même les lecteurs vidéo HTML5. Cela permet aux streamers d'atteindre une audience aussi large que possible.
-	SRT (Secure Reliable Transport): est un autre protocole open source développé par le fournisseur de technologie de streaming Haivision, les principaux avantages pour lesquels SRT est connu sont la sécurité, la fiabilité, la compatibilité et le streaming à faible latence.
-	MPEG-PASH: est l'un des derniers protocoles de streaming, mis au point par le Moving Pictures Expert Group (MPEG), en alternative à la norme HLS.
-	WebRTC : est un open source qui vise à offrir la diffusion en continu avec une latence en temps réel. Développé à l'origine pour les applications basées sur le chat et l'utilisation VoIP
## Conclusion [[C]](https://cefre-uahb.fr.gd/LA-VISIOCONFERENCE.htm#:~:text=Une%20seule%20séance%20de%20visioconférence,la%20perte%20de%20temps%20supprimée.)
Une seule séance de visioconférence permet d'éviter les appels téléphoniques, les courriels, les télécopies, les envois de courrier ou les pires déplacements.
Grâce à la vidéoconférence, les communications s'améliorent, la prise de décisions est facilitée, la productivité augmente, les coûts diminuent et le temps perdu est éliminé.
## Bibliographie:
1. [Que signifie visioconférence](https://trueconf.com/video-conferencing-architecture.html)
 
 - Auteur et date d'expédition : Inconnu
 - Date de consultation : 20 Juillet 2022
 - Avis : l'explicaion est un peu court
 - Résume : Explication sur visioConférence et ses protocoles
 
2. [Pourquoi la visioconférence ?](https://www.a2com.fr/blog/6-bonnes-raisons-dutiliser-la-visioconference-en-entreprise/#:~:text=Le%20collaboratif%20est%20au%20cœur,dans%20une%20entreprise%20multi%20sites.)
 
 - Auteur et date d'expédition : Inconnue, 2021
 - Date de consultation : 27 juillet 2O22
 - Avis : Le Résumer trés courte
 - Résume: Explication des avantages de la visioConférence 
 
3. [Principaux protocoles](https://fr.acervolima.com/protocoles-de-visioconference/)
 
 - Auteur et date d'expédition : Jack Sparrow, 5 juillet de 2022
 - Date de consultation : 27 juillet 2022
 - Avis : très court explication
 - Résume : explication des principaux protocoles de la visioconférence
 
4. [Differentes normes de visioConférence](https://www.dwpro.fr/blog/visioconference/normes-visioconference#:~:text=De%20plus%2C%20la%20norme%20H,les%20systèmes%20de%20visioconférence%204K.)
 
 - Auteur et date d'expédition
 - Date de consultation : 27 juillet 2022
 - Avis : Explications sont bien détailler mais peu de contenu
 - Résume : Explication sur les normes de visioConférence
 
5. [Difference entre la visioConférence et le streaming video](https://www.avast.com/fr-fr/c-what-is-streaming)
 
 - Auteur et date d'expédition : Alfred Poor, le 28 septembre 2019
 - Date de consultation : 2 Aout 2022
 - Avis : Les types de streaming sont bien expliqué
 - Résume : Explication et comparaison de visioconférence et streaming vidéo
 
6. [Protocoles streaming video](https://www.cdnetworks.com/media-delivery-blog/video-streaming-protocols/)
 
 - Auteur et date d'expédition : Inconnu , December 19, 2021
 - Date de consultation : 8 Aôut 2022
 - Avis : Bien detailler et bien complet avec les explications
 - Résume : Differentes protocole du streaming video
 
7.Autre liens:
 - https://fr.acervolima.com/protocoles-de-visioconference/
 - https://www.cisco.com/c/fr_ca/support/docs/quality-of-service-qos/qos-packet-marking/21662-video-qos.html
 - https://docplayer.fr/508783-Deploiement-de-la-visioconference-ip-dans-un-etablissement-etat-de-l-art-et-evolution-des-protocoles.html
