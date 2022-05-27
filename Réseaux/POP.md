---
layout: default
title: POP
parent: Réseaux
---

# POP

## qu'est ce que POP

Le protocole POP (Post Office Protocol) est un protocole beaucoup utiliser dans le monde du mail. Il permet de télécharger nos mails se situant sur un serveurs vers notre ordinateur sur lequelles on ne pourra les lires via un client de messagerie telle qu'Outlook, Thunderbird, Gmail, etc. Après avoir téléchargé un mail avec POP, celui-ci ne se trouvera plus sur le serveur.

Le gros point négatif de POP vient de là. Après avoir consulté ses mails sur une machine, il est alors impossible pour l'utilisateur de consulter ce même mail sur une autre machine.

Il est est possible de conserver ses mails sur le serveur POP, mais pendant une durée limitée qui doit être configuré sur le client mail de l'utilisateur.

Le protocole Imap, son concurrant, quant à lui permet de télécharger ses mails à partir d'un serveur mail, mais le mail en question reste sur le serveur. Cela permet au utilisateur de consulter leurs emails sur n'importe quelle autre machine. 

Le protocole POP peut s'occuper de l'authentification de l'utilisateur et donc de la vérification de notre identifiant et de notre mot de passe. Il bloque également l'accès à notre boite aux lettres lorsque nous y accédons et empêche alors toute autre connexion.

[[https://www.kalanda.net/images/faq/imap/pop-ou-imap.png]]

### Le protocole POP gère les commandes suivantes :

* LIST : donne le nombre de courriers présents sur le serveur avec leur numéro.
* RETR [numéro] : récupère le courrier numéro en attente sur votre serveur.
* DELE [numéro] : détruit le courrier numéro.
* NOOP : vérifie la connexion.
* LAST : récupère le dernier message arrivé sur le serveur.
* QUIT : quitte la session et en autorise une autre.

## Ou est-il utilisé ?

Tous les clients mails peuvent être paramètrés afin d'utiliser POP

## Qu'elle est le futur du protocole POP

POP4 n'existe qu'en tant que proposition informelle ajoutant la gestion de base des dossiers, la prise en charge des messages en plusieurs parties, ainsi que la gestion des indicateurs de message pour concurrencer IMAP. 
Cependant, son développement n'a pas progressé depuis 2003.

De l'autre côté, son concurent IMAP publie sa dernière version, la version 4, en 1994 et aucune version 5 n'a été annoncer jusqu'à aujourd'hui.

## L'avantage du protocole POP

* POP est plus simple à utilisé, plus simple à configurer et efficace
* Permet la gestion de ses emails en local ce qui facilite et augmente la vitesse de recherche et de tri.
* Il nécessite une quantité minimale de ressources afin de fonctionner.

## Désaventage du protocole POP

* Nécessite une connexion constante à internet.
* Oblige l'utilisateur à sauvegarder ses mails afin de ne pas les perdre en cas de problème.

## Source

* Fonctionnement du protocole POP et IMAP - Base de connaissances - KALANDA. (2019). Kalanda.Net. Consulté le 27 mai 2022, à l’adresse https://www.kalanda.net/apps/index.php/knowledgebase/27/Fonctionnement-du-protocole-POP-et-IMAP.html#:%7E:text=Le%20protocole%20POP%20permet%20de,les%20voir%20sur%20mon%20smartphone).

* Arobase.org. (2018, 25 septembre). Le protocole POP. Consulté le 25 mai 2022, à l’adresse https://www.arobase.org/fonctionnement/pop.htm#:%7E:text=Le%20protocole%20POP%20g%C3%A8re%20l,m%C3%AAme%20temps%20%C3%A0%20votre%20courrier.

* Questiaux, J. (2021, 18 juin). Mon compte email : choisir POP ou IMAP ? Better Web. Consulté le 20 mai 2022, à l’adresse https://www.betterweb.fr/blog/mon-compte-email-choisir-pop-ou-imap

* A. (2020, 31 décembre). POP3, Post Office Protocol : de quoi s’agit-il, à quoi sert-il et en quoi est-il différent d’IMAP? Informatique Mania. https://www.informatique-mania.com/en/courriers-electroniques/bureau-de-protocole-de-messagerie-pop3/

* Wikipedia contributors. (2022c, mai 16). Post Office Protocol. Wikipedia. https://en.wikipedia.org/wiki/Post_Office_Protocol

* POP et IMAP : différences, avantages et inconvénients. (2010, 17 mars). L’Orient-Le Jour. https://www.lorientlejour.com/article/650498/POP_et_IMAP%2B%253A_differences%252C__avantages_et_inconvenients.html#:%7E:text=Parmi%20les%20avantages%20du%20compte,enfin%20une%20utilisation%20minimale%20des