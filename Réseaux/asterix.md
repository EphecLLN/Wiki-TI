---
layout: default
title: Asterix
parent: Réseaux
---
# ASTERIX

## **Qu'est-ce que ASTERIX ?[^9]**

Astérix est une implémentation logicielle d'un autocommutateur privé PBX qui
est une centrale téléphonique desservant une organisation privée et permettant
le partage des lignes réseaux d'un bureau central entre les téléphones installés
en interne.

Chaque Appareil connecté au PBX possède un numéro de téléphone d'extension
désigné qui peut ou non être mappé au plan de numérotation du bureau central.

Astérisk permet donc l'établissement et le contrôle de la communication entre
les différents terminaux tels que les postes téléphoniques, les appareils ou
services de voix sur protocole internet (VoIP).

###### Fig.1 Schéma d'une infrastructure asterisk

![flux d'appel](https://user-images.githubusercontent.com/74672498/169313118-e214baf6-f8d7-40f4-a8c8-4b757a64b2e8.png)

###### "https://docplayer.fr/508846-Le-support-de-la-video-par-asterisk.html" docplayer.fr

Il prend en charge plusieurs fonctionnalités disponibles dans les systèmes PBX
notament la messagerie vocale, conférence téléphonique, distribution
automatique d'appel. Ce qui fait l'une des plus grande force d'Asterisk est que
les utilisateurs peuvent créer de nouvelles fonctionnalités en écrivant des
scripts de plan de numérotation dans plusieurs des propres langages d'
extensions d'Asterisk tout simplement en ajoutant des modules chargeables personnalisés écrits
en PHP ou C. Qu'en est-il réellement de l'intégration des flux video dans une
infrastructure Asterisk ?

## **Flux video**

### Introduction[^2]

Encore appelé streaming, c'est un processus d'envoie de contenu en direct. La
plupart des logiciels de voix sur ip telles que facebook, skype discord intègre
des flux multimédia : appel video. En générale la VoIP est utilisée pour décrire
des communications point à point. Pour ce qui est du flux video par
visioconférence on parle de communication multipoint qui est une diffusion d'un
emetteur vers un groupe de récepteur.

Pour ce faire, les deux protocoles utilisés dans une infrastructure Astérisk
pour permettre ce flux video sont RTP et RTCP

### **Protocole RTP**

Le principe du protocole RTP (Real-Time Transfert Protocol) consiste à envoyer
les paquets en temps réel sur le réseau. Les paquets sont marqués temporellement
de manière à être réordonnancé par le client afin d’afficher la vidéo de
manière cohérente. Il permet de founir un moyen uniforme pour le transport de
données sur IP soumises à des contraintes de temps réel tels que les flux média,
audio et vidéo.

#### **Caractéristiques[^3]**

RTP étant utilisé en mode unidirectionnel (d'un emetteur à un récepteur), il
peut être aussi utilisé en mode multicast via satellite sans garantie de qualité de service ([QoS](https://fr.wikipedia.org/wiki/Qualit%C3%A9_de_service)). Les données sont augmentées de l'ajout du protocole de contrôle RTCP.

Le rôle principal de RTP consiste à numéroter les paquets IP afin de
reconstituer les flux voix ou vidéo de manière fluide et ce, même si le réseau
sous jacent change l'ordre des paquets. Par opposition à HTTP et FTP qui fonctionnent au-dessus de TCP, RTP fonctionne au dessus de UDP

Il est utilisé par SIP, H.323, MGCP et éventuellement d'autres protocoles pour transporter les médias entre les points d'extrémité.

Exemple de communication via SIP

###### Fig.2 Schéma d'une converstion via SIP

![utilisation_avec_sip](https://user-images.githubusercontent.com/74672498/169355709-eaec4f3c-978d-42bf-8bc2-6c2245cf71ea.png)

###### "https://www.researchgate.net/figure/VoIP-call-setup-based-on-SIP-SDP-RTP-RTCP-protocols-based-on-9_fig3_1924445" researchgate.net

- **Utilisation avec un canal de retour[^3]**

RTP peut être utilisé conjointement avec un canal de retour via RTCP voir RTSP(Real Time Streaming Protocol destiné aux système de streaming média). Ce canal de retour peut servir à demander des changements de compression ou de débit pour les applications multimedias ou encore informer l'émetteur des propriétés temps-réel du canal.

Pour améliorer les performances de RTP, un protocole spécifique au streaming permet de contrôler la diffusion du contenu, il s’agit de RTSP (Real Time Streaming Protocol).

RTSP est un protocole de niveau applicatif qui sert à contrôler les propriétés temps-réel du contenu délivré. Il est adapté aussi bien à la diffusion de données pré-enregistrées que de données diffusées en direct.

- **Utilisation en mode unicast[^3]**

 Ce mode de diffusion nécessite l'ouverture d’un flux spécifique entre le serveur et le client. Les protocoles utilisés sont RTSP pour le contrôle du flux et RTP pour l’émission du flux. RTSP utilise TCP alors que RTP utilise UDP. L’intérêt de RTSP par rapport à RTP est d’ajouter un contrôle sur le contenu et de pouvoir par exemple accéder directement à un point donné du contenu sans avoir à le télécharger dans sa globalité. Il améliore ainsi les performances.

Comment sa se passe :

1. Le client contacte le serveur de streaming grâce au protocole RTSP.
2. le serveur retourne via RTSP une description de la session de streaming qu’il va ouvrir.
3. Le serveur informe le client du nombre de flux ( Une session étant composé de plusieurs flux : audio, video etc)
4. Le serveur donne des informations décrivant les flux comme le type du média et le codec de compression
5. Les flux sont diffusés séparément via le protocole RTP.

###### Fig.2 Utilisation de RTP en unicast

![rtp_unicast](https://user-images.githubusercontent.com/74672498/169299420-0e91d1bf-7c4f-429f-99c5-da9a9a9e5b5c.png)

###### "https://www.iifa.fr/video-ip" iifa.fr

- **Utilisation en mode multicast[^2]**

La mise en œuvre de RTP en mode multicast requiert une configuration préalable de
routage au niveau du récepteur, qui doit faire lui-même la demande de routage à
ses routeurs hôtes, entre l'émetteur et le récepteur. L'émetteur quant à lui
informe séparément les routeurs de diffusion auxquels il est directement
connecté. il n'existe qu'un seul flux qui est dupliqué au niveau de chaque recepteur. on peut prendre le cas de la diffusion d'une chaine de TV où le flux est partagé entre tous les recepteurs.

Comment sa se passe :

Pour se connecter au multicast, le client doit télécharger un fichier type SDP(Session Description Protocol) qui contient les informations nécessaires pour recevoir le flux multicast, l'adresse IP du serveur, le numéro de port et les informations de description des flux (même informations que celle envoyées par RTSP dans le cas d'un diffusion unicast).

###### Fig.3 Utilisation de RTP en multicast

![rtp_multicast](https://user-images.githubusercontent.com/74672498/169301654-924eb1f4-ccb3-4ee8-aed4-38f244f8cc9f.png)

###### "https://www.iifa.fr/video-ip" iifa.fr

#### **RTP et la NAT[^7]**

 Les protocoles de signalisation utilisés pour les échanges multimédias (H.323 SIP, MGCP etc) sont en effet sensible à la NAT! Pourquoi?

 Lors d'une signalisation, ces protocoles ne se contentent pas de mentionner leur adresse IP dans l'entête des paquets qu'ils envoient mais l'indiquent également dans le corps de leurs messages. Par exemple avec le protocole SIP un message d'invitation INVITE comporte des informations sur l'ip de la source du paquet. Le récepteur ne peut répondre correctement à la requête puisque l'ip source initiale est une adresse privée. Le récepteur envoie donc sa réponse vers l'adresse ip source spécifiée qui ne lui est pas accessible, et le paquet de réponse n'arrive jamais.

###### Fig.4 Utilisation de la NAT

 ![nat_rtp](https://user-images.githubusercontent.com/74672498/169309816-8f15135e-f6b8-4e7b-8c0c-acfcd0bcd053.png)

###### "https://docs.switzernet.com/3/public/110303-asterisk-nat/" docs.switzernet.com

 Le protocole NAT posent donc quelques soucis au niveau d'une infrastructure décentralisée notamment lors de l'échange des paquets voix et video dans le protocole RTP. RTP étant décentralisé, pour résoudre le problème, tous les paquets RTP seront envoyés de l'interlocuteur au serveur avant d'être redirigés vers le bon client. Cette solution induit une forte utilisation de la bande passante du serveur, mais permet de résoudre le problème de la NAT.

#### **Intégration de RTP dans Asterisk[^4]**

La configuration du protocole RTP dans une infrastructure Asterisk passe par le fichier rtp.conf qu'Asterisk utilise pour générer et recevoir le traffic RTP. Par défaut rtp.conf utilise la plage de ports RTP comprise entre 10000 à 20000.

Pour chaque appel SIP bidirectionnel entre 02 terminaux, cinq ports sont généralement utilisés : 5060 pour la signalisation SIP, Un port pour le flux de données un port pour RTCP dans une direction, 2 ports supplémentaires pour le flux de données et RTCP dans la direction opposée.

Exemple

```
;
; Configuration RTP
;
[général]
;
; RTP start et RTP end configurent les adresses de début et de fin
;
rtpstart=10000
rtpend=20000
```

Si vous avez un NAT ou un pare-feu entre Asterisk et le serveur, vous devez les configurer pour gérer le transfert des ports.

### **Intégration de la video dans Asterisk[^5]**

Asterisk prend en charge une variété de support audio et vidéo et fournit des modules CODEC pour faciliter l'encodage et le décodage des flux audio. Pour l'instant le transcodage vidéo ou le transcodage d'image n'est pas pris en charge.

#### **Pris en charge des vidéos[^6]**

|  Non  | Valeur de configuration | Format Module | Distribué avec Astérisk |
| :----: | :---------------------: | :-----------: | :-----------------------: |
| H.261 |          h261          |      n/A      |            OUI            |
| H.263 |          h263          |  format_h263  |            OUI            |
| H.263+ |          h263p          |  format_h263  |            OUI            |
| H.264 |          h264          |  format_h264  |            OUI            |

le fichier produit par les pilotes de format vidéo Asterisk n'est pas dans un format vidéo habituel. [Gstreamer](https://gstreamer.freedesktop.org/) prend en charge la production de ces fichiers et la conversion de divers fichiers vidéo en fichiers vidéo + audio Asterisk.

#### **Prise en charge du pilote de canal[^8]**

Dans Asterisk un canal est l'unité atomique qui transporte un appel au sein d'Asterisk. Il peut s'agir d'une connection physique à une ligne téléphonique ou à un poste téléphonique comme il peut s'agir d'une connection logique induite par un appel en provenance d'un réseau de données. Chaque canal est géré par un pilote qui en connaît les moindres détails et qui n'expose que le strict nécessaire à la couche supérieure du PBX.

Généralement, un pilote de Canal lit dans un fichier de configuration les informations concernant le matériel/protocole qu'il doit gérer puis entre en attente de modification d'état sur les canaux physiques/logiques. Dès qu'un changement d'état survient, (Sonnerie par exemple), le pilote crée une structure de données de type channel et y attache tous les callbacks nécessaires à la communication.

Les pilotes de canal qui prennent en charge la video dans Asterisk sont:

| Pilote de canal |    Module    |                                Remarque                                |
| :-------------: | :-----------: | :---------------------------------------------------------------------: |
|       SIP       |  chan_sip.so  |     Le pilote de canal SIP (chan_sip.so) prend en charge la vidéo     |
|      IAX2      | chan_iax2.so |        Prend en charge les appels vidéo (sur les troncs aussi)        |
|      Local      | chan_local.so |          Transfère les appels vidéo en tant que canal proxy          |
|      Agent      | chan_agent.so |          Transfère les appels vidéo en tant que canal proxy          |
|       oss       |  chan_oss.so  | Prend en charge l'affichage/le décodage vidéo, voir video_console.txt |

Ci-dessous on peut voir un exemple de configuration de base de sip : sip.conf

```conf
[general]
context=Hell
srvlookup=yes
tos_sip=cs3
tos_audio=ef
tos_video=af41
useragent=AUFSIPUA Pwrd by Asterisk PBX 
externip = 1.2.3.4
externhost=foo.baz.bar
localnet=192.168.0.0/255.255.0.0
localnet=10.0.0.0/255.0.0.0
localnet=172.16.0.0/12
localnet=169.254.0.0/255.255.0.0

[tom]
type=friend
secret=s3cr3t
qualify=10 
disallow=all
allow=speex
allow=gsm
allow=alaw
nat=no 
host=dynamic
canreinvite=no
context=hackers


        reload chan_sip.so ou sip reload
        sip list objects
        sip list peers
        sip list settings|domains|channels|subscriptions
        sip list peers
```

Pour ce qui est des applications, elles rendent des services lorsqu'un appel est traité dans le système. Pour chaque appel, une suite d'applications est exécuté en série dans l'ordre dans lequel elles sont défini dans un diaplan (fichier qui reprend les instructions de traitement d'un l'appel). Certaines applications utilisent un fichier de configuration comme l'application VoiceMail (Messagerie vocale)

Les applications de plans de numérotation qui sont connus pour gérer la vidéo sont :

- Messagerie vocale - Stockage de la messagerie vocale vidéo
- Enregistrer - Enregistre les fichiers audio et vidéo
- Lecture - Lit une vidéo tout en étant invité à lire l'audio
- Echo - Renvoie l'audio et la vidéo à l'utilisateur

## Bibliographie

[^1]: "_Qu'est-ce que ASTERIX ?_", fr.wikipedia.org, https://fr.wikipedia.org/wiki/Asterisk_(logiciel) (consulté le 18/05/2022)
       ** Résumé :  Bref définition d'asterisk ainsi que de ses fonctionnalités
       ** Avis sur la ressource : Très leger et pas assez explicite dans certains point
    
[^2]: "_RTP ?_", iifa.fr, https://www.iifa.fr/video-ip) (consulté le 19/05/2022)
       ** Résumé :  Détaille les différents mode d'utilisation de RTP
       ** Avis sur la ressource : Clair et schématique le meilleur pour comprendre RTP en unicast et multicast
    
[^3]: "_RTP ?_", fr.wikipedia.org, https://fr.wikipedia.org/wiki/Real-time_Transport_Protocol (consulté le 19/05/2022)
       ** Résumé :  Définition de RTP ainsi que des protocoles associés
       ** Avis sur la ressource : Va droit au but : Bonne base pour comprendre le protocole
    
[^4]: "_Intégration de RTP dans Asterisk_", asteriskdocs.org, http://www.asteriskdocs.org/en/2nd_Edition/asterisk-book-html-chunk/asterisk-APP-D-SECT-37.html (consulté le 18/05/2022)
       ** Résumé :  Montre une configuration minimaliste de RTP au sein d'astérisk
       ** Avis sur la ressource : Clair mais assez leger : insuffisant pour une configuration total
    
[^5]: "_Prise en charge des pilote de canal_", wiki.asterisk.org, https://wiki.asterisk.org/wiki/display/AST/Video+Telephony (consulté le 19/05/2022)
       ** Résumé :  Donne les différents pilote de canal ainsi que les format video d'asterisk
       ** Avis sur la ressource : Assez concis
    
[^6]: "_Prise en charge des vidéos_", fr.wikipedia.org, https://wiki.asterisk.org/wiki/display/AST/Asterisk+Audio+and+Video+Capabilities (consulté le 19/05/2022)
       ** Résumé :  Document officiel d'asterisk (Prise en charge des médias)
       ** Avis sur la ressource : Pas très détaillé concernant les flux videos
    
[^7]: "_RTP et la NAT_", fr.wikipedia.org, https://wiki.asterisk.org/wiki/display/AST/Asterisk+Audio+and+Video+Capabilities (consulté le 19/05/2022)
       ** Résumé :  Fonctionnement d'Asterisk avec un routeur NAT et les problèmes rencontrés
       ** Avis sur la ressource : Super explication de comment résoudre le problème de la nat dans une infrastructure Asterisk
    
[^8]: "_Pilote de canal_", wiki.auf.org, https://wiki.auf.org/wikiteki/Asterisk/QuelquesNotions (consulté le 19/05/2022)
       ** Résumé :  Détaille les différents cannaux et leurs pilote utilisés par Astérisk
       ** Avis sur la ressource : Super article très explicite
    
[^9]: "_PABX_", fr.wikipedia.org, https://fr.wikipedia.org/wiki/Autocommutateur_t%C3%A9l%C3%A9phonique_priv%C3%A9 (consulté le 19/05/2022)
       ** Résumé :  Bref définition de ce que c'est qu'un PBX et de son evolution
       ** Avis sur la ressource : Chouette définition
