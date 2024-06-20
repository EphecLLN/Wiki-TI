---
layout: default
title: L'injection d'infos falsifiées dans le DNS
parent: Réseaux
---

# L'injection d'infos falsifiées dans le DNS

## Introduction

L'injection d'informations falsifiées dans le DNS (DNS Spoofing) consiste à réaliser une attaque qui trompe l’utilisateur final en lui faisant croire qu’il communique avec un nom de domaine légitime alors qu’en réalité il communique avec un nom de domaine ou une adresse IP que l’attaquant a mis en place. [1] <br>
Il y a 2 techniques qui permettent d'atteindre ce résultat : 
* Le DNS ID spoofing
* Le DNS cache poisoning

## ID spoofing

Lorsqu'une machine A veut communiquer avec une machine B mais que celle-ci (A) ne connait que son nom et pas son adresse IP, elle doit envoyer une requête au serveur DNS du réseau de la machine B. Cette requête possède un numero d' identification (ID) que le serveur utilise lors de sa réponse à la machine A. Les attaquants utilisent ce fait pour envoyer une fausse réponse avant le serveur DNS et rediriger le trafic à l'endroit voulu. 
Cependant, pour trouver l'ID de la requête DNS qui varie de 0 à 65535, l'attaquant doit utiliser un sniffer si il est sur le même réseau, ou en utilisant une faille des systèmes d'exploitation ou prédire l'ID suite à l'envoi de plusieurs requêtes et l'analyse des réponses. [2] [3]

## Cache poisoning

Le DNS cache poisoning est une attaque visant à corrompre le cache des serveurs DNS, qui contient temporairement les correspondances entre les noms de domaine et leurs adresses IP. Voici de manière simple les étapes nécessaire à l'attaque :

Le serveur DNS dispose d'un cache pour stocker les correspondances entre noms de machine et adresses IP, afin d'accélérer la résolution DNS.<br>
Un pirate souhaite effectuer une attaque de DNS cache poisoning en ayant sous son contrôle un nom de domaine et son serveur DNS.<br>
Le pirate envoie une requête au serveur DNS cible, demandant la résolution d'un nom de machine du domaine contrôlé par le pirate.<br>
Le serveur DNS cible transmet la requête au serveur DNS du pirate, car ce dernier a l'autorité sur le domaine demandé.<br>
Le serveur DNS du pirate répond à la requête avec une réponse correcte, mais il ajoute également des enregistrements additionnels falsifiés (par exemple, un nom de machine lié à l'adresse IP du pirate).<br>
Les enregistrements additionnels falsifiés sont stockés dans le cache du serveur DNS cible.<br>
Lorsqu'une autre machine effectue une requête pour résoudre le même nom de machine, le serveur DNS cible renvoie l'adresse IP falsifiée du pirate au lieu de l'adresse IP réelle.<br>

Ainsi, les utilisateurs finaux qui accèdent au site ou au service concerné sont redirigés vers l'adresse IP contrôlée par le pirate, ce qui peut entraîner diverses conséquences malveillantes. [2] [3]

### Détails 

Au lieu de recourir au protocole TCP, qui requiert un processus de "handshake" entre les parties en communication pour initier l'échange et vérifier l'identité des dispositifs, les requêtes et réponses DNS utilisent le protocole UDP.
Dans le cadre de l'UDP, aucune garantie n'est établie quant à l'ouverture d'une connexion, à la préparation du destinataire à recevoir les données, ou à l'authenticité de l'expéditeur. Cette vulnérabilité découle de la nature même de l'UDP, puisqu'il ne met pas en place des mécanismes vérifiant l'intégrité des échanges. 
Les données d'en-tête peuvent ainsi être altérées, ce qui permet à un attaquant d'envoyer un message via UDP en prétendant qu'il provient d'un serveur légitime.

Si un résolveur DNS reçoit une réponse falsifiée, il l'accepte et la place en mémoire cache sans procéder à une vérification rigoureuse, car il ne dispose pas des moyens pour confirmer l'exactitude ni la légitimité de l'information.
Le DNS a été conçu à une époque où seules les universités et les centres de recherche étaient connectés à Internet. 
À cette période, il n'était pas envisagé que des individus malveillants tentent de propager de fausses informations DNS.

Malgré les vulnérabilités inhérentes au mécanisme de mise en cache du DNS, les attaques d'empoisonnement du DNS ne sont pas aisées à réaliser. 
Comme le résolveur DNS interroge en réalité le serveur de noms autoritaire, les attaquants disposent de seulement quelques millisecondes pour envoyer une réponse falsifiée avant que la réponse réelle du serveur de noms autoritaire ne soit reçue. [5]

## Dangers potentiels

Ce genre d'attaque n'est généralement pas dans le but de copier de simples sites et fournir de fausses informations aux utilisateurs mais plutôt de se faire passer pour des sites qui collectent des données personnelles importantes comme un site banquaire ou un site de vente et de recupérer les données sensibles des utilisateurs pour s'en servir. [4]

Certains cybercriminels sont connus pour altérer votre connexion Internet, vous dirigeant ainsi vers des pages saturées de publicités dans le but de générer des profits en facturant les réseaux publicitaires. 
Ils peuvent cibler la redirection pour n'affecter que les publicités qui sont chargées sur des sites Web légitimes. 

Dans certaines nations, des serveurs DNS personnalisés sont employés pour restreindre l'accès à des sites web au sein de leurs frontières. 
Dans ces pays, lorsque les citoyens tentent d'atteindre un site bloqué par les autorités, ils sont automatiquement redirigés vers d'autres destinations (généralement vers des sites « autorisés »).
[6]

## Où se réalise le DNS Spoofing ? 

La cible principale est un endroit doté d'un réseau wifi public gratuit mais n'importe quel endroit doté d'appareils connectés est susceptible d'être attaqué. En effet, c'est surtout les réseaus wifi public gratuit qui ne sont pas très bien configurés et sécurisés. [1] [4]
Mais il y a évidemment d'autres méthodes ; en voici quelques unes. <br> 
Les méthodes courantes de détournement de DNS comprennent l’exploitation de portails captifs dans des hotspots WiFi payants, où les requêtes DNS sont redirigées vers un serveur de paiement jusqu’à ce que l’accès soit acheté. 
Changer le paramètre du serveur DNS d’un utilisateur pour un serveur DNS contrôlé par un attaquant est une autre tactique, permettant à l’attaquant de fournir de fausses adresses IP pour les sites Web demandés ou de capturer des données envoyées à des sites légitimes.

L’accès non autorisé aux données DNS faisant autorité est également une méthode, impliquant souvent le vol de mots de passe, l’exploitation des vulnérabilités du système ou d’autres techniques intelligentes. 

Les attaques homographiques jouent sur les similitudes entre les noms de domaine avec différentes polices ou encodages. Par exemple, en utilisant les majuscules "I" pour ressembler à un "L" minuscule dans un nom de domaine, incitant les utilisateurs à penser que c’est un site légitime.

Les attaques par navigateur impliquent JavaScript ou des astuces de navigateur qui masquent la barre d’URL, affichant une URL différente de celle du site visité. Cela peut induire les utilisateurs en erreur en leur faisant croire qu’ils se trouvent sur un site différent de ce qu’ils sont réellement.
Un exemple de cela est l’attaque "Inception Bar" ciblant les navigateurs mobiles Chrome, qui affiche une fausse barre d’URL pour les utilisateurs. [1]

## Comment se protéger du DNS Spoofing ?

En tant qu'utilisateur, il est recommandé de ne pas introduire de données personelles sur des sites lorsqu'on utilise un réseau public qui n'est pas digne de confiance.

En tant que propriétaire d'un serveur DNS, il est nécessaire d'utiliser DNSSEC qui ajoute une signature cryptographique aux entrées requises par les résolveurs avant qu'ils n'acceptent les consultations DNS comme authentiques.
Le protocole DNS standard n'implique pas de chiffrement et ne fournit pas de mécanismes intrinsèques pour s'assurer de l'authenticité des modifications et des consultations effectuées par des serveurs et utilisateurs légitimes.
Pour pallier cela, DNSSEC introduit une couche de sécurité supplémentaire en ajoutant des signatures aux processus, permettant ainsi la vérification des mises à jour et empêchant toute tentative d'usurpation du DNS. 
DNSSEC a gagné en popularité récemment, particulièrement dans le contexte des réseaux Wi-Fi publics, où l'usurpation de noms de domaine pourrait compromettre la confidentialité des données des utilisateurs. [4]

Un utilisateur qui se connecte à un réseau public douteux peut aussi utiliser un VPN pour que toutes ses communications soient chiffrées, pas uniquement ses requêtes DNS. [6]

## Bibliographie

[1] https://www.infoblox.com/dns-security-resource-center/what-are-dns-spoofing-dns-hijacking-dns-cache-poisoning/#:~:text=DNS%20Spoofing%20refers%20to%20any,%2Dthe%2Dmiddle%20style%20attack., infoblox, consulté le 28/08/23, date article : pas de date
[2] https://inetdoc.net/guides/tutoriel-secu/tutoriel.securite.attaquesprotocoles.dns.html, inetdoc, consulté le 28/08/23, date article : pas de date
[3] https://www.securiteinfo.com/attaques/hacking/dnsspoofing.shtml, securiteinfo, consulté le 28/08/23, date article : 16 Juillet 2001
[4] https://www.proofpoint.com/fr/threat-reference/dns-spoofing, proofpoint, consulté le 28/08/23, date article : pas de date
[5] https://www.cloudflare.com/fr-fr/learning/dns/dns-cache-poisoning/, cloudflare, consulté le 28/08/23, date article : pas de date
[6] https://www.avg.com/fr/signal/what-is-dns-hijacking, AVG, consulté le 28/08/23, date article : 22 February 2017