---
layout: default
title: SMB
parent: Réseaux
---

# SMB (Server Message Block)

Dans une époque où les réseaux informatiques sont partout, aussi bien chez soi qu’au travail ou bien même dans la rue, la création 
d’un réseau local comme alternative à Internet fait l’unanimité. Ceci permet au membre du réseau d’échanger toute sorte d’information 
et donnée entre eux, mais également, offre la possibilité de gérer des serveurs, des imprimantes ou des routeurs par exemple. Pour 
gérer tout ça, de nombreuses règles sont mise en œuvre par le billet de protocoles, l’un d’entre eux est le SMB.¹

## Qu’est-ce que le SMB ? ¹

Le SMB ou Server Message Block est un protocole développé en 1983 par IBM et en constante évolution depuis lors. Ce dernier est un 
protocole serveur-client gérant l’accès à des fichiers, à des répertoires complets et à d’autres ressources du réseau tel que des 
imprimantes ou des interfaces partagées sur le réseau, de plus ce dernier a également été créer dans le but d’échanger des 
informations avec les autres protocoles, c’est la communication inter-processus.

Mis à disposition d’un large public dans le cadre de l’implémentation du système d’exploitation réseau OS/2 LAN Manager ainsi que de 
son successeur LAN Server, ce protocole majoritairement utilisé sous Windows qui supporte la rétrocompatibilité (compatible avec 
d’anciennes versions du système d’exploitation) pour le SMB. Certains logiciels permettent par ailleurs d’utiliser le Server Message 
Block avec les systèmes d’exploitation Linux et Unix grâce à Samba par exemple et ainsi de permettre la communication 
multi-plateforme. ²

## Comment fonctionne SMB ? 

Le protocole SMB permet à des membres d’un même réseau ayant également le protocole d’implémenté d’accéder à des fichiers et services 
partagés à ces fins sur celui-ci. Pour commencer une connexion entre les différent parti doit être mise en place, SMB utilise, pour 
ce faire le Transmission Control Protocol (TCP) prévu avec un handshaking en trois temps entre le client et le serveur avant 
l’établissement de la connexion, pour la suite du transfert de données les spécifications de TCP sont également maintenue durant 
l’échange de données.¹

![image](https://user-images.githubusercontent.com/71373028/172682323-972f15e0-cfb7-4aee-ad37-0288bee85706.png)

Lors d’une connexion SMB via un protocole TCP, des demandes par messages sont effectuées du côté serveur et client pour mettre des 
fichiers ou service à disposition sur le réseau.²

## Comment le SMB a-t-il évolué depuis sa création ?¹

Publié en 1983, SMB a subi de nombreuses modifications par rapport à sa première version, partant de la première version, SMB 1.0, 
nous sommes maintenant et depuis l’arrivée de Windows 10 à SMB 3.1.1.
Voici les grandes étapes qui ont fait évoluer ce protocole :
- SMB 1.0 (CIFS) :
Assimilé dans un premier temps à la modification Common Internet File System (CIFS), ce n’est qu’une partie de la première édition de
SMB. En effet, cet aspect du protocole n’était porté que sur les machines équipées de Windows NT 4.0 et ne communiquait uniquement 
via l’interface NetBIOS grâce aux port UDP 137 et 138 respectivement utiliser pour la résolution de nom et la transmission de paquet,
mais aussi en TCP via le port 139 utilisé pour l’établissement de la connexion et du transfert. Il nous faudra donc attendre 
l’arrivée de Windows 2000 pour voir disparaître la dépendance de NetBIOS, mais aussi une connexion directe via le port TCP 445, 
encore utiliser aujourd’hui.

- SMB 2.0¹ :
Arrivé en 2006 avec Windows Vista, il apporte, avec lui d’importante réforme sur le protocole précédant. Les principales 
améliorations du système étant les suivantes :
- la réduction drastique du nombre de commandes
- l’optimisation des performances grâce à une file d’attente des requêtes
- la compatibilité avec les liens symboliques
- amélioration des signatures des messages
- plus de modularité avec un accroissement du nombre de clients
En plus de ça, Microsoft permet malgré tout de continuer la communication avec les versions précédente, mais également avec d’autres 
systèmes d’exploitation différente.

- SMB 2.1¹ :
Cette version, liée à Windows 7 propose une révision du système précédant dès 2007, en plus des légères améliorations apporté par 
celui-ci, il implémente une nouvelle méthode de verrouillage afin de réguler plus facilement l’accès aux fichiers.

- SMB 3.0¹ :
Arrivé en 2012 avec Windows 8 le server message block se voit aussi attribuer une nouvelle version. Avec cette dernière version, 
l’accent est porté sur la performance et la sécurité des connexions. Celle-ci apporte aussi avec elle la possibilité d’accéder au 
stockage des fichiers à distance grâce à RDMA (Remote Direct Memory Access). Avec sa refonte, des nouvelles fonctionnalités sont 
mises en place telle que la possibilité d’établir des connexion multi channels, mais aussi un chiffrement de bout en bout lors du 
transfert de fichier.

- SMB 3.1.1¹ :
Publier en 2015 avec Windows 10 cette version augmente la sécurité avec un contrôle de l’intégrité avant l’authentification grâce à 
un hachage SHA 512, elle oblige également à sécuriser la connexion des appareils communiquant avec le protocole SMB 2.0.
Microsoft viendra par la suite stopper la communication par défaut vers le SMB 1 pour des raisons de sécurité, cette option est 
toutefois possible à activer pour permettre la rétrocompatibilité. 

## Comment se protéger lors de l’utilisation d’un SMB ?²

Étant donné que Microsoft a accordé énormément d’importance au fait de maintenir la communication entre les différentes version du 
protocole, mais ceci s’accompagne d’énormément de risques. Effectivement, la communication entre les vieilles et les nouvelles 
versions engendre de nombreuses failles de sécurité rendant les utilisateurs vulnérable notamment aux attaques DoS.
Les risques sont d’autant plus élevés pour les attaques via SMB dans les réseaux ou tous les protocoles du protocole sont 
généralement activés afin de permettre son utilisation par les appareils qui y ont recours comme les imprimantes ou autres appareils 
en réseaux. Si malgré tous les utilisateurs classiques n’utilise plus les anciennes versions de SMB, les hackers, eux, se voient leur 
travail facilité par les failles laisser dans les premières versions, c’est pour ça que Windows 10 permet de désactiver le suivi des 
premières versions.

## Quelques cas d’utilisation et d’implémentation des Server Message Block¹ ²

L’utilisation la plus fréquente est bien entendu la connexion client-serveur entre les ordinateurs et les serveurs de fichiers, mais 
pas que, car le protocole est également capable de communiqué avec d’autres processus et comprend un simple échange de données entre 
deux appareils aussi.
En-dehors de son utilisation classique sous Windows, de nombreux logiciels ont fini par intégrer le Protocole de Server Message Block 
dans leurs projets afin de permettre son utilisation sur des systèmes indépendants à Windows tel que Linux, Unix ou encore Mac en 
voici quelques exemples :
- Samba : sans aucun doute l’une des implémentations de SMB parmi les plus connues et rependue en dehors de Windows. Ce logiciel 
libre a permis la communication Server Message Block sur les systèmes Unix et Linux dès 1991.³ ⁶
- Netsmb : développé pour les systèmes d’exploitation BSD, il est une implémentation du client et du serveur SMB directement sur le 
noyau de celui-ci. Premièrement exploiter sur le système FreeBSD 4.4, il est, à ce jour, disponible sur un bon nombre de systèmes BSD 
tel que NetBSD et macOS.¹
- YNQ : précédemment appeler NQ, utilisé dans les systèmes non dotés de Windows, il implémente le Server Message Block et permet de 
ce fait la communication avec les appareils équipés de Windows. Il est développé par l’entreprise Visuality Systèms Ltd, une 
entreprise spécialisée dans les logiciels.⁴
- ConnectedNas : développée spécialement pour les appareils Android par Connected Way, cette application permet au utilisateur 
d’appareil mobile d’échanger facilement des données avec des appareils SMB.
Pour des raisons de sécurité, tous les protocoles cités utilisent des versions avancées du service SMB afin de garantir la sécurité 
des utilisateurs.⁵


## Bibliographie

* [^1] : INOS, (https://www.ionos.fr/digitalguide/serveur/know-how/server-message-block-smb/#:~:text=SMB%20(Server%20Message%20Block)%20est,interfaces%20partag%C3%A9es%20dans%20le%20r%C3%A9seau), 24/09/2020, consulté le 14/05/2022

     **Résumé** : Présentation globale des SMB
       **Avis sur la ressource** : Il s'agit d'un article détaillant le protocole SMB datant de 2020, mais je n'ai pas vu de mise à 
       jour plus récent.

* [^2] : Article communautaire, MICROSOFT, (https://docs.microsoft.com/en-us/windows-server/storage/file-server/smb-security), 17/02/2022,consulté le 15/05/2022

       **Résumé** : Description générale de SMB 
       **Avis sur la ressource** : Il s'agit d'un article communautaire mis à jour régulièrement et surveiller par Microsoft pour 
       assurer la fiabilité de la ressource.

* [^3] : Article communautaire, WIKIPEDIA, (https://fr.wikipedia.org/w/index.php?title=Samba_(informatique)&oldid=193049431), 21/04/2022, 25/05/2022
       
       **Résumé** : Description de Samba 
       **Avis sur la ressource** : Cet article reprend les grandes lignes de samba, il faut toutefois se méfier de certains articles Wikipédia qui peuvent être modifiés par tout le monde.

* [^4] : VISUALITYNQ, (https://visualitynq.com/products/ynq/), 2022, consulté le 27/05/2022 

       **Résumé** : Description et explication de YNQ
       **Avis sur la ressource** : C'est un article officiel sur le protocole permettant de comprendre grâce a des exemples de son fonctionnement 

* [^5] : Article communautaire, TRUENAS, (https://www.truenas.com/docs/core/coretutorials/sharing/smb/smbshare/), 22/05/2022, consulté le 06/06/2002

       **Résumé** : Description du protocole et sur quoi il se base 
       **Avis sur la ressource** : C'est un article mis à jour régulièrement avec comme dernière version le 22/05

* [^6] : SAMBA, (https://www.samba.org/),02/05/2022, Consulté le 25/05/2022 

       **Résumé** : Documentation officiel Samba
       **Avis sur la ressource** : Documentation très intéressante aussi bien pour ce renseigner que pour configurer Samba, il est en plus mis à jour régulièrement 


