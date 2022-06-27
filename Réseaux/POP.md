---
layout: default
title: POP
parent: Réseaux
---
# POP

## Qu'est ce que POP [1] [2]

Le protocol POP (Post Office Protocol) est un protocole frotement utilisé dans le monde du mail. Il permet de télécharger nos mails se situant sur un serveur vers notre ordinateur sur lequels on pourra les lire via un client de messagerie tel qu'Outlook, Thunderbird, Gmail, etc. Après avoir téléchargé un mail avec POP, celui-ci ne se trouvera plus sur le serveur.

Le gros point négatif de POP vient de là. Après avoir consulté ses mails sur une machine, il est alors impossible pour l'utilisateur de consulter ce même mail sur une autre machine.

Il est possible de conserver ses mails sur le serveur POP, mais pendant une durée limitée qui doit être configuré sur le client mail de l'utilisateur.

Le protocole Imap, son concurrent, quant à lui permet de télécharger ses mails à partir d'un serveur mail, mais le mail en question reste sur le serveur. Cela permet à l'utilisateur de consulter leurs emails sur n'importe quelle autre machine.

Le protocole POP peut s'occuper de l'authentification de l'utilisateur et donc de la vérification de notre identifiant et de notre mot de passe. Il bloque également l'accès à notre boite aux lettres lorsque nous y accédons et empêche alors toutes autres connexions.

## Conversation POP [9]

<p align="center">
    <img width="380" alt="Conversation POP" src="../Assets/Images/conversation%20POP3.PNG">
</p>


### Le protocole POP gère les commandes suivantes [5] :

* LIST : donne le nombre de courriers présents sur le serveur avec leur numéro.
* RETR [x] : récupère le courrier numéro en attente sur votre serveur.
* DELE [x] : détruit le courrier numéro.
* NOOP : vérifie la connexion.
* LAST : récupère le dernier message arrivé sur le serveur.
* QUIT : quitte la session et en autorise une autre.

## Où est-il utilisé ?

Tout les clients mails peuvent être paramétrés afin d'utiliser POP.

## Quel est le futur du protocole POP [5]

POP4 n'existe qu'en tant que proposition informelle ajoutant la gestion de base des dossiers, la prise en charge des messages en plusieurs parties, ainsi que la gestion des indicateurs de message pour concurrencer IMAP.
Cependant, son développement n'a pas progressé depuis 2003.

De l'autre côté, son concurrent IMAP a publié sa dernière version, la version 4, en 1994 et aucune version 5 n'a été annoncée jusqu'à aujourd'hui.

## L'avantages du protocole POP [3][4]

* POP est plus simple à utiliser, plus simple à configurer et efficace
* Permet la gestion de ses emails en local ce qui facilite et augmente la vitesse de recherche et de tri.
* Il nécessite une quantité minimale de ressources afin de fonctionner.

## Désavantages du protocole POP [3][4]

* Nécessite une connexion constante à internet.
* Oblige l'utilisateur à sauvegarder ses mails afin de ne pas les perdre en cas de problème.

## Sécurisation du protocole POP [10]

La sécurisation du protocole POP, peut-être faites de deux manière s
différentes.

Premièrement, on peut sécuriser les échanges au niveau du protocole grâce
à un certificat SSL ou TLS. Cette technique permet aux serveurs et aux
clients de s'authentifier mutuellement. Les échanges de messages peuvent
alors être chiffrés.

Ensuite, il est possible d'ajouter une couche de sécurité PGP et S/MIME.
Ces 2 outils sécurisent le dialogue entre les utilisateurs de la
messagerie électronique et sur des échanges de bout en bout.
Pour garder l'intégrité et la confidentialité des échanges PGP et S/MIME
fonctionnent à l'aide d'un mécanisme de signature numérique et de
chiffrement.

## Source

* [1] Fonctionnement du protocole POP et IMAP - Base de connaissances - KALANDA. (2019). Kalanda.Net. Consulté le 27 mai 2022, à l’adresse https://www.kalanda.net/apps/index.php/knowledgebase/27/Fonctionnement-du-protocole-POP-et-IMAP.html#:%7E:text=Le%20protocole%20POP%20permet%20de,les%20voir%20sur%20mon%20smartphone).
* [2] Arobase.org. (2018, 25 septembre). Le protocole POP. Consulté le 25 mai 2022, à l’adresse https://www.arobase.org/fonctionnement/pop.htm#:%7E:text=Le%20protocole%20POP%20g%C3%A8re%20l,m%C3%AAme%20temps%20%C3%A0%20votre%20courrier.
* [3] Questiaux, J. (2021, 18 juin). Mon compte email : choisir POP ou IMAP ? Better Web. Consulté le 20 mai 2022, à l’adresse https://www.betterweb.fr/blog/mon-compte-email-choisir-pop-ou-imap
* [4] A. (2020, 31 décembre). POP3, Post Office Protocol : de quoi s’agit-il, à quoi sert-il et en quoi est-il différent d’IMAP? Informatique Mania. https://www.informatique-mania.com/en/courriers-electroniques/bureau-de-protocole-de-messagerie-pop3/
* [5] Wikipedia contributors. (2022c, mai 16). Post Office Protocol. Wikipedia. https://en.wikipedia.org/wiki/Post_Office_Protocol
* [6] POP et IMAP : différences, avantages et inconvénients. (2010, 17 mars). L’Orient-Le Jour. https://www.lorientlejour.com/article/650498/POP_et_IMAP%2B%253A_differences%252C__avantages_et_inconvenients.html#:%7E:text=Parmi%20les%20avantages%20du%20compte,enfin%20une%20utilisation%20minimale%20des
* [7] POP3 - RFCs 1939 : https://www.ietf.org/rfc/rfc1939.txt
* [8] IMAP4 - RFCs 3501 : https://datatracker.ietf.org/doc/html/rfc3501
* [9] POP3-Client-Server-Procedure. (2009). Researchgate. https://www.researchgate.net/figure/POP3-Client-Server-Procedure_fig2_251917023
* [10] Problématique. (2003). Techniques de l’Ingénieur. https://www.techniques-ingenieur.fr/base-documentaire/technologies-de-l-information-th9/securite-des-si-services-et-applications-42315210/securite-des-e-mails-pgp-et-s-mime-h5330/problematique-h5330niv10001.html
