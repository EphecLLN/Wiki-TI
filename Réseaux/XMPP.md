---
layout: default
title: XMPP
parent: Réseaux
---

# XMPP

## Qu'est-ce que XMPP ?[^1] [^2] [^3] [^4] [^5]

XMPP (Extensible Messaging and Presence Protocol) est un protocole connu principalement pour la messagerie instantanée en cross-platform,
mais il est aussi utilisé bien que cela soit moins connu pour de l'échange de données entre applications systèmes, mais également pour de la supervision système et réseau ou du cloud computing.<br>

XMPP est connus pour avoir défini les standards pour la communication en message instantanée en 1999 et début des années 2000.

Au jour d'aujourd'hui il est encore utilisé principalement par WhatsApp, mais également google , GNOME, IBM, ...<br>
Ce protocole est construit en language XML (Extensible Markup Language) parfois appeler "Jabber" alors que celui-ci est un service baser sur XMPP.
XMPP utilise TCP/IP avec une architecture client-server transmettant ces informations avec des paquets XML.

X: Pour extensible car c'est un projet open source qui est tout le temps modifier et améliorer.<br>
M: Pour messages car il a très bon mécanisme d'envoi de messages.<br>
P: Pour presences car il détecte l'état d'une personne si elle est en ligne , déconnecter ou occuper.<br>
P: Pour protocole car c'est un protocole de communication tout simplement.<br>

## Histoire[^1] [^2] [^3]

En 1998 Jeremie Miller invente Jabber un protocole de message instantané utilisant le XML. Mais il manquait un framework pour le faire fonctionner et c'est pour 
trouver une alternative Open Source au protocole propriétaire existant que XMPP va etre développé.<br> Il va être dévoloppé par la communauté Jabber et dans un premier temps
etre nommée Jabber protocole (un alias encore utilisé par certain).<br> Il va être reconnus en 2004 par l'IETF et adopter le nom de XMPP. <br>
Il va être adopté successivement par Apple en 2005 et google de 2005 à 2013 puis par Facebook de 2008 à 2015 puis par Skype en 2011 puis par WLM en 2011.<br>
La dernière release de l'IETF est le RFC7622 avec la version la plus moderne de XMPP en 2015.

## Fonction principale[^3] [^4]

* Recevoir et envoyer des messages en direct entre utilisateurs
* Blocker un utilisateur
* Gérer ses contacts 
* Gérer les fils de discussion suivit ou non
* Transmettre le status de l'utilisateur (En ligne, occuper, ...)

## Comment cela fonctionne ?
XMPP est divisé en plusieurs parties au milieu de tout cela se trouve XMPP Core:
### XMPP Core
le coeur est composé des features suivantes:
* un flux d'envoi de données XML
* un canal sécurisé avec TLS
* Authentification avec SASL
* utilisation de l'UTF-8
* Autorisation pour une discussion bidirectionnelle
* Des infromations sur la disponibilité & la présence

### Jingle
Jingle est la version XMPP de transmission VoIP. Il transmettra leur paquet au travers de XMPP mais utilise tout de même les protocoles RTP(Real-time Transport Protocol).<br>
Jingle a été développer pour supporter SIP (Session Initiation Protocol) et SDP (Session Description Protocol) ce qui permet de passer facilement des réseaux XMPP à des réseaux SIP.

### Multi-User-Chat (MUC)
Comme son nom l'indique c'est une extension qui permet de créé des conversations avec plusieurs utilisateurs ainsi que de crée des cannals de discussion. Dans ces features nous retrouvons : 
* Inviter des utilisateurs
* Exclure des utilisateurs
* bannir des utilisateurs
* Mettre en place des mots de pass
* Donner des droits au modérateur

### BOSH

BOSH (Bidirectional-streams Over Synchronous HTTP) c'est en soi une version plus efficace du protocole TCP.<br> Il est néanmoins uniquement utilisé pour la communication entre XMPP serveur et client.

![](https://i.goopics.net/mr699q.png)

### Client-server

Comme vous venez de le voir sur le schéma au-dessus XMPP est basé sur un architecture server-client. <br>
Vous envoyer donc vos donner XML de votre client vers le serveur et le serveur va rediriger vers le client destinataire de données.<br>
Chaque client est représenté par un identifiant unique que le serveur va utiliser pour transmettre les messages à bonne destination.<br>
XMPP utilise des gateways quand il doit envoyer des informations ou recevoir des informations d'un réseau qui n'est pas XMPP (Ex: SMS, SMTP, IRC).

### Connexion TCP

En temps normal XMPP va établir une connexion TCP persiste pour ne pas devoir en refaire une à chaque message envoyé.<br>
Dans les versions plus récentes XMPP utilise des WebSockets et/ou TLS pour une communication sécurisée.

### Envoie de messages

XMPP utilise l'envoi de messages Asynchrones pour permettre aux utilisateurs d'envoyer autant de message qu'ils le désirent sans attendre de réponse.  <br>
Et les 2 utilisateurs n'ont pas besoin d'être connecté en même temps pour envoyer des messages. <br>
XMPP contrairement aux autres offres une expérience de discussion presque instantanée car contrairement aux autres qui vont ping le serveur toutes les x secondes (processus appellé "polling") pour récupérer des data.<br>
XMMP lui va directement envoyer tout nouveau message de l'utilisateur vers le serveur et du serveur vers le destinataire.

### Décentraliser

XMPP n'as pas de serveur officiel c'est-à-dire que n'importe qui peut créé son serveur et faire ce qu'il veut avec.

## Avantages et Inconvénients[^1] [^3]
<br>

|Avantages|Inconvénients|
|---|---|
|Open Source le code et document officiel sont accessibles gratuitement| Il est difficil de transferer ces utilisteur d'un autre réseau (MSN, AIM, etc), mauvaise migration |
|Base validée par l'IETF et continue d'être mis à jour et il est stable | Fonctionalité incomplet présente comme Jingle qui n'est pas exploitable en l'état|
|Décentraliser||
|Sécurité disponible (ex:TLS, SASL)||
|Flexible il peut être utilisé pour autre chose que les messages instantanés||
|Confidentiallité: peut mettre en place OMEMO, OpenPGP, ... ||
|Supporte beaucoup de language ( C, C++, C#, Ruby, Java, ...)||

## Conclusion

XMPP est la base des protocoles de message instantané et cela vaut donc la peine de l'étudier pour le comprendre et choisir quel chemin emprunté dans le cas où l'ont
voudrais développer une app avec de la messagerie instantanée. <br>
Il est bien sur aussi tout à fait exploitable et à beaucoup d'espace pour pouvoir grandir la team XSF travail sur le développement de l'encryptage pour une communication plus sécurisé. <br>
Dans le futur que ce soit du coté VoIP ou encore dans le domaine de l'IoT. <br>
Car je le rappelle il n'est pas limité qu'a la messagerie instantanée XMPP c'est plus que ça.<br>

## Bibliographie
[^1]: "XMPP", xmpp.org, https://xmpp.org/about/technology-overview/ (consulté le 25/05/2022) 
   ** Résumé :  Explication détaillée sur XMPP et ces features
   ** Avis sur la ressource : Très bonne ressource et fiable car c'est le site officiel
[^2]: "Extensible Messaging and Presence Protocol", fr.wikipedia.org, https://fr.wikipedia.org/wiki/Extensible_Messaging_and_Presence_Protocol (consulté le 25/05/2022) 
   ** Résumé :  Histoire de XMPP et présentation technique
   ** Avis sur la ressource : Certains points sont légers mais en général très complet
[^3]: "XMPP Refresher: The Open Instant Messaging Protocol Then & Now", getstream.io, https://getstream.io/blog/xmpp-extensible-messaging-presence-protocol/ (consulté le 25/05/2022) 
   ** Résumé :  Explication détaillé sur XMPP , ces features , coté technique et perspective pour le futur
   ** Avis sur la ressource : Très Complet 
[^4]: "XMPP Protocol", www.geeksforgeeks.org, https://www.geeksforgeeks.org/xmpp-protocol/ (consulté le 25/05/2022) 
   ** Résumé :  Bref explication sur XMPP et ce qu'il représente
   ** Avis sur la ressource : Très léger et pas assez d'explication
[^5]: "Extensible Messaging and Presence Protocol (XMPP)", www.techopedia.com, https://www.techopedia.com/definition/396/extensible-messaging-and-presence-protocol-xmpp (consulté le 25/05/2022) 
   ** Résumé :  Explication de ce qu'est XMPP
   ** Avis sur la ressource : Pas complet et très léger

