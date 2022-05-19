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
###### Fig.1 Schéma d'une infrastructure asterisk
![flux d'appel](https://user-images.githubusercontent.com/74672498/169313118-e214baf6-f8d7-40f4-a8c8-4b757a64b2e8.png)
###### "https://docplayer.fr/508846-Le-support-de-la-video-par-asterisk.html" docplayer.fr

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

#### Caractéristiques

RTP étant utilisé en mode unidirectionnel (d'un emetteur à un récepteur), il
peut être aussi utilisé en mode multicast via satellite.

Le rôle principale de RTP consiste à numéroter les paquets IP afin de
reconstituer les flux voix ou vidéo de manière fluide et ce, même si le réseau
sous jacent change l'ordre des paquets.

Il est utilisé par SIP, H.323, MGCP et éventuellement d'autres protocoles pour transporter les médias entre les points d'extrémité.

- Utilisation avec un canal de retour

RTP peut être utilisé conjointement avec un canal de retour via RTCP voir RTSP(Real Time Streaming Protocol destiné aux système de streaming média). Ce canal de retour peut servir à demander des changements de compression ou de débit 

###### Fig.2 Utilisation de RTP en unicast
![rtp_unicast](https://user-images.githubusercontent.com/74672498/169299420-0e91d1bf-7c4f-429f-99c5-da9a9a9e5b5c.png)

###### "https://www.iifa.fr/video-ip" iifa.fr

- Utilisation en mode multicast

La mise en œuvre de RTP en mode multicast requiert la configuration préalable de
routage au niveau du récepteur, qui doit faire lui-même la demande de routage à
ses routeurs hôtes, entre l'émetteur et le récepteur. L'émetteur quant à lui
informe séparément les routeurs de diffusion auxquels il est directement
connecté. il n'existe qu'un seul flux qui est dupliqué au niveau de chaque recepteur.

Pour se connecter au multicast, le client doit télécharger un fichier type SDP(Session Description Protocol) qui contient les informations nécessaires pour recevoir le flux multicast, l'adresse IP du serveur, le numéro de port et les informations de description des flux. 
######  Fig.3 Utilisation de RTP en multicast
![rtp_multicast](https://user-images.githubusercontent.com/74672498/169301654-924eb1f4-ccb3-4ee8-aed4-38f244f8cc9f.png)

######  "https://www.iifa.fr/video-ip" iifa.fr

 #### RTP et la NAT
 
 Les protocoles de signalisation utilisés pour les échanges multimédia dont H.323 SIP, MGCP et bien d'autres sont dit sensible à la NAT.
 
 Lors d'une signalisation, ces protocoles ne se contentent pas de mentionner leur adresse IP dans  l'entête des paquets qu'ils envoient mais indiquent également dans le corps de leurs messages. Par exemple avec le protocole SIP un message d'invitation INVITE comporte des informations sur l'ip de la source du paquet. Le récepteur ne peut répondre correctement à la requête puisque l'ip source initiale est une adresse privée. Le récepteur envoie donc sa réponse vers l'adresse ip source spécifiée qui ne lui est pas accessible, et le paquet de réponse n'arrive jamais.
 
 ###### Fig.4 Utilisation de la NAT 
 ![nat_rtp](https://user-images.githubusercontent.com/74672498/169309816-8f15135e-f6b8-4e7b-8c0c-acfcd0bcd053.png)
###### "https://docs.switzernet.com/3/public/110303-asterisk-nat/" docs.switzernet.com 
 Le protocole NAT posent donc quelques soucis au niveau d'une infrastructure décentralisée notamment lors de l'échange des paquets voix dans le protocole RTP. RTP étant décentralisé, pour résoudre le problème, tous les paquets RTP seront envoyés de l'interlocuteur au serveur avant d'être redirigés vers le bon client. Cette solution induit une forte utilisation de la bande passante du serveur, mais permet de résoudre le problème de la NAT



#### Intégration de RTP dans Asterisk

La configuration du protocole RTP dans une infrastructure Asterisk passe par le fichier rtp.conf qu'Asterisk utilise pour générer et recevoir le traffic RTP. Par défaut rtp.conf utilise la plage de ports RTP comprise entre 10000 à 20000

Pour chaque appel SIP bidirectionnel entre 02 terminaux, cinq ports sont généralement utilisés : 5060 pour la signalisation SIP, Un port pour le flux de données un port pour RTCP dans une direction, 2 ports supplémentaires pour le flux de données et RTCP dans la direction opposée.


### Protocole RTCP

C'est un protocole de contrôle des flux RTP, permettant de récupérer des
informations sur les participants d'une session, et sur la qualité de service en
transmettent périodiquement des paquets de contrôles.
