---
layout: default
title: roots servers
parent: Réseaux
---


# Introduction :

Dès que l'on recherche un élément en ligne, que l'on communique avec un service en ligne, le serveur racine (du DNS) joue un rôle important dans la résolution de nom. Comment, sur base d'une adresse IP, il est possible d'arriver sur Google, Youtube ou encore Facebook?

## Histoire :

Avant le DNS, la résolution de nom  sur internet se faisait grâce à un fichier texte que l'on recopiait sur les ordinateurs, par transfert de fichier.

Ce fichier portait le nom de: HOSTS.TXT(5).

En 1982, ce système montre ses premières limites, et plusieurs propositions de remplacement voient le jour. Les premières idées ne sont pas gardées. Un certain Paul Mockapetris(6) fut désigné responsable quant au développement d'un autre système de résolution de nom.
Il est responsable du DNS, une architecture qu'il proposera en 1983 dans les RFC 882 et RFC 883.

Ce dernier est également l'inventeur du premier serveur de courrier électronique basé sur le protocole SMTP.

## Explications des root-servers

Afin de nous rendre sur nos sites web préférés, nous utilisons la résolution de nom DNS, qui passe par les serveurs racines (roots-servers).

Un serveur racine est un élément primordial pour la résolution de nom. Il répond aux requêtes ou demandes des clients dans la zone racine du DNS (Cette zone marque le niveau le plus élevé, au premier niveau de l'espace de nom DNS).
Ces derniers n'exécutent pas eux-même la résolution de nom, mais informent à la place le client des autres serveurs DNS à consulter de manière plus précise afin d'obtenir une meilleure réponse. (1)

Cependant, afin de pouvoir interroger un serveur racine, il faut également posséder des informations à son propos. Quel est l'adresse IP du serveur racine responsable de la zone que je cherche à joindre?
Pour ce faire, il existe un fichier, appelé "fichier de zone racine", qui contient tous les noms et toutes les adresses IP des domaines de premier niveau.
Ce fichier est téléchargé lors de l'installation d'un client DNS, comme Bind9 par exemple. 
Ce dernier se trouve dans le repertoire /etc/bind, sous le nom de db.root. (2)

La gestion de base du serveur racine est à la responsabilité de l'ICANN (Internet Corporation for Assigned Names and Numbers).

Il n'y a plus, contrairement à la croyance populaire, uniquement 13 serveurs racines uniques du DNS .
En effet, nous devons plutôt parler de 13 "identités de serveur". Ces "identités", ayant chacune une adresse IP, sont souvent considérées comme des serveurs racines.

Lettre des serveurs racines du DNS | Adresse IPv4 | Adresse IPv6 | Opérateurs 
--- | ------------- | ---------- | -------------- 
A | 198.41.0.4 | 2001:503:ba3e::2:30 | VeriSign | 
B | 192.228.79.201 | 2001:478:65::53 | USC-ISI |
C | 192.33.4.12 | 2001:500:2::c | Cogent Communications |
D | 199.7.91.13 | 2001:500:2d::d | 	University of Maryland |
E | 192.203.230.10 | | NASA |
F | 192.5.5.241 | 2001:500:2f::f | 	ISC |
G | 192.112.36.4 | | U.S. DoD NIC |
H |	128.63.2.53 | 2001:500:1::803f:235 | US Army Research Lab |
I |	192.36.148.17 | 2001:7FE::53 | 	Autonomica |
J | 192.58.128.30 | 2001:503:c27::2:30 | 	VeriSign |
K |	193.0.14.129 | 2001:7fd::1 | RIPE NCC |
L | 199.7.83.42 | 2001:500:3::42 | 	ICANN |
M | 202.12.27.33 | 2001:dc3::35 | 	WIDE Project |

(4)

Lors d'une tentative de résolution de nom, on va tout d'abord interroger un serveur racine, afin d'obtenir plus d'informations sur le domaine de premier niveau (TLD, Top Level Domain).

Par la suite, grâce à la réponse du TLD, nous allons interroger le SOA de la zone recherchée, afin d'obtenir l'IP recherchée. (1)

C'est de cette manière que nous parvenons à accéder aux contenus de certains sites. Sans le fichier db.root, toute la résolution de nom du DNS global ne marcherait pas. Les serveurs DNS racines sont les piliers du DNS.

## Attaques sur les serveurs racines :

Il y a déjà eu, durant son histoire, de nombreuses attaques sur les serveurs racines. Étant la base de la résolution de nom, si le service tombe en panne, c'est tout internet qui tombe en panne. (4)

### Attaque de 2002 :

Lors de cette attaque, 7 serveurs sur les 13 ont été touchés, et on vu leurs performances dégradées à cause de cette dernière. Les auteurs de cette attaque l'ont effectué à l'aide d'un DDoS. Ils ont réussi à générer un volume de requêtes 40x supérieurs à la normale.
Suite à cette attaque, le systeme anycast a été mis en place :

"Anycast est une technique d'adressage et de routage permettant de rediriger les données vers le serveur informatique le « plus proche » ou le « plus efficace » selon la politique de routage."
[source](https://fr.wikipedia.org/wiki/Anycast).

### Attaque de 2007 :

Les serveurs F, G, L et M ont été attaqués pendant 24h.

Lors de cette attaque, l'impact sur M a été moindre grâce à anycast.

5000 machines, essentiellement basées en corée du sud, ont générées du traffic en direction des Etats-Unis. (4)

### Attaque de 2015 : 

5 millions de requêtes par seconde prennant la route vers les 13 serveurs racines. L'attaque était destinée à chaque serveur racine de chaque lettre. La durée totale de cette attaque n'a pas dépassé 4 heures, mais les serveurs DNS racines ont été confrontés à environ 1 trillion de requêtes erronées. (3)

## Conclusion :

Les root-servers / serveurs racines sont un élément essentiel à la résolution de nom sur internet. Ils représentent le fondement du fonctionnement du service DNS. Sans eux, nous ne pourrions plus aller sur un site sur base de son url. Ce service a, de nombreuses fois, été attaqué pour essayer de le faire tomber, mais ils n'y sont jamais arrivés.
Le service est super robuste, pas étonnant vu son importance dans la société à l'heure actuelle.


## Bibliographie:


### source 1 :  

[URL](https://www.ionos.fr/digitalguide/serveur/know-how/quest-ce-quun-serveur-racine-definition-et-fonctionnement/) de l'article

Auteur : Know-how

Titre de l'article : Qu’est-ce qu’un serveur racine ? Définition et fonctionnement

Date de consultation : 25 mai 2023

### source 2 :  

[URL](https://linux.developpez.com/formation_debian/serveur-dns.html)
Auteur : ?

Titre de l'article : Chapitre 9. Monter un serveur DNS

Date de consultation : 25 mai 2023

### source 3 :  

[URL](https://news.sophos.com/fr-fr/2015/12/12/serveurs-dns-racine-attaque-ddos-massive/) de l'article

Auteur : Sophos France

Titre de l'article : Des serveurs DNS racine résistent à une attaque DDoS massive

Date de consultation : 25 mai 2023

### source 4 :  

[URL](https://fr.wikipedia.org/wiki/Serveur_racine_du_DNS) de l'article

Auteur : ?

Titre de l'article : Serveur racine du DNS

Date de consultation : 25 mai 2023

### source 5 :  

[URL](https://fr.wikipedia.org/wiki/Serveur_racine_du_DNS](https://www.techno-science.net/glossaire-definition/Domain-Name-System-page-2.html)) de l'article

Auteur : [liste ici](http://fr.wikipedia.org/w/index.php?title=Domain%20Name%20System&action=history)

Titre de l'article : Domain Name System

Date de consultation : 25 mai 2023

### source 6 :  

[URL](https://fr.wikipedia.org/wiki/Paul_Mockapetris) de l'article

Auteur : [ici]([http://fr.wikipedia.org/w/index.php?title=Domain%20Name%20System&action=history](https://fr.wikipedia.org/wiki/Utilisateur:Bot_de_pluie))

Titre de l'article : Paul Mockapetris

Date de consultation : 25 mai 2023


