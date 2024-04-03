---
layout: default
title: XMPP
parent: Réseaux
---

# XMPP

## Qu'est-ce que XMPP ?[^1] [^2] [^3] [^4] [^5]

XMPP (Extensible Messaging and Presence Protocol) est un protocole connu principalement pour la messagerie instantanée en cross-platform,
mais il est aussi utilisé bien que cela soit moins connu pour de l'échange de données entre applications systèmes, mais également pour de la supervision système et réseau ou du cloud computing.<br>

XMPP est connu pour avoir défini les standards pour la communication en message instantané en 1999 et début des années 2000.

Au jour d'aujourd'hui, il est encore utilisé principalement par WhatsApp, mais également google , GNOME, IBM, ...<br>
Ce protocole est construit en language XML (Extensible Markup Language) parfois appeler "Jabber" alors que celui-ci est un service baser sur XMPP.
XMPP utilise TCP/IP avec une architecture client-server transmettant ces informations avec des paquets XML.

X: Pour extensible car c'est un projet open source qui est tout le temps modifié et amélioré.<br>
M: Pour messages car il a un très bon mécanisme d'envoi de messages.<br>
P: Pour présence car il détecte l'état d'une personne si elle est en ligne , déconnectée ou occupée.<br>
P: Pour protocole car c'est un protocole de communication tout simplement.<br>

## Histoire[^1] [^2] [^3]

En 1998 Jérémie Miller invente Jabber un protocole de message instantané utilisant le XML. Mais il manquait un framework pour le faire fonctionner et c'est pour 
trouver une alternative Open Source au protocole propriétaire existant que XMPP va être développé.<br> Il sera développé par la communauté Jabber et dans un premier temps être nommé Jabber protocole (un alias encore utilisé par certain).<br> Il sera reconnu en 2004 par l'IETF et il adopte le nom de XMPP. <br>
Il sera utilisé successivement par Apple en 2005 , google de 2005 à 2013 ensuite par Facebook de 2008 à 2015 puis par Skype en 2011 et enfin par WLM en 2011.<br>
La dernière release de l'IETF est le RFC7622 avec la version la plus moderne de XMPP en 2015.

## Fonction principale[^3] [^4]

* Recevoir et envoyer des messages en direct entre utilisateurs
* Blocker un utilisateur
* Gérer ses contacts 
* Gérer les fils de discussion qui sont suivis ou non
* Transmettre le status de l'utilisateur (En ligne, occupé, ...)

## Comment cela fonctionne ?
XMPP est divisé en plusieurs parties au milieu de tout cela se trouve XMPP Core:
### XMPP Core[^6] [^7]
le coeur est composé des features suivantes:
* un flux d'envoi de données XML
* un canal sécurisé avec TLS
* Authentification avec SASL
* utilisation de l'UTF-8
* Autorisation pour une discussion bidirectionnelle
* Des informations sur la disponibilité & la présence

### Jingle
Jingle est la version XMPP de transmission VoIP. Il transmettra leur paquet au travers de XMPP mais utilise tout de même les protocoles RTP(Real-time Transport Protocol).<br>
Jingle a été développé pour supporter SIP (Session Initiation Protocol) et SDP (Session Description Protocol) ce qui permet de passer facilement des réseaux XMPP à des réseaux SIP.

### Multi-User-Chat (MUC)
Comme son nom l'indique c'est une extension qui permet de créé des conversations avec plusieurs utilisateurs ainsi que de créer des canaux de discussion. Dans ces features nous retrouvons : 
* Inviter des utilisateurs
* Exclure des utilisateurs
* bannir des utilisateurs
* Mettre en place des mots de passe
* Donner des droits au modérateur

![](https://i.goopics.net/mr699q.png)

### Client-server

Comme vous venez de le voir sur le schéma au-dessus XMPP est basé sur une architecture server-client. <br>
Vous envoyez donc vos données XML de votre client vers le serveur et le serveur redirige vers le client destinataire de données.<br>
Chaque client est représenté par un identifiant unique que le serveur utilisera pour transmettre les messages à bonne destination.<br>
XMPP utilise des gateways quand il doit envoyer des informations ou recevoir des informations d'un réseau qui n'est pas XMPP (Ex: SMS, SMTP, IRC).

### Connexion TCP

En temps normal XMPP va établir une connexion TCP persiste pour ne pas devoir en refaire une à chaque message envoyé.<br>
Dans les versions plus récentes XMPP utilise des WebSockets et/ou TLS pour une communication sécurisée.

### Envoie de messages

XMPP utilise l'envoi de messages Asynchrones pour permettre aux utilisateurs d'envoyer autant de message qu'ils le désirent sans attendre de réponse.  <br>
Et les 2 utilisateurs n'ont pas besoin d'être connectés en même temps pour envoyer des messages. <br>
XMPP contrairement aux autres, offre une expérience de discussion presque instantanée car contrairement aux autres qui vont ping le serveur toutes les x secondes (processus appellé "polling") pour récupérer des data.<br>
XMMP lui enverra directement tout nouveau message de l'utilisateur vers le serveur et du serveur vers le destinataire.

### Décentraliser

XMPP n'as pas de serveur officiel c'est-à-dire que n'importe qui peut créé son serveur et faire ce qu'il veut avec.

## Avantages et Inconvénients[^1] [^3]
<br>

|Avantages|Inconvénients|
|---|---|
|Open Source le code et document officiel sont accessibles gratuitement| Il est difficile de transférer ces utilisteurs d'un autre réseau (MSN, AIM, etc), mauvaise migration |
|Base validée par l'IETF et continue d'être mis à jour et il est stable | Fonctionalité incomplète présente comme Jingle qui n'est pas exploitable en l'état|
|Décentralisé||
|Sécurité disponible (ex:TLS, SASL)||
|Flexible il peut être utilisé pour autre chose que les messages instantanés||
|Confidentiallité: peut mettre en place OMEMO, OpenPGP, ... ||
|Supporte beaucoup de language ( C, C++, C#, Ruby, Java, ...)||

## Conclusion

XMPP est la base des protocoles de messages instantanés et cela vaut donc la peine de l'étudier pour le comprendre et choisir quel chemin emprunté dans le cas où l'on
voudrait développer une app avec de la messagerie instantanée. <br>
Il est bien sûr tout à fait exploitable. Il a beaucoup d'espace pour pouvoir grandir. La team XSF travaille sur le développement de l'encryptage pour une communication plus sécurisée. <br>
Dans le futur,il sera développé que ce soit du côté VoIP ou encore dans le domaine de l'IoT. <br>
Car je le rappelle il n'est pas limité qu'à la messagerie instantanée XMPP c'est plus que ça.<br>

## Bibliographie
[^1]: "XMPP", xmpp.org, https://xmpp.org/about/technology-overview/ (consulté le 25/05/2022) 
   ** Résumé :  Explication détaillée sur XMPP et ces features
   ** Avis sur la ressource : Très bonne ressource et fiable car c'est le site officiel
[^2]: "Extensible Messaging and Presence Protocol", fr.wikipedia.org, https://fr.wikipedia.org/wiki/Extensible_Messaging_and_Presence_Protocol (consulté le 25/05/2022) 
   ** Résumé :  Histoire de XMPP et présentation technique
   ** Avis sur la ressource : Certains points sont légers mais en général très complet
[^3]: "XMPP Refresher: The Open Instant Messaging Protocol Then & Now", getstream.io, https://getstream.io/blog/xmpp-extensible-messaging-presence-protocol/ (consulté le 25/05/2022) 
   ** Résumé :  Explication détaillée sur XMPP , ces features , côté technique et perspective pour le futur
   ** Avis sur la ressource : Très Complet 
[^4]: "XMPP Protocol", www.geeksforgeeks.org, https://www.geeksforgeeks.org/xmpp-protocol/ (consulté le 25/05/2022) 
   ** Résumé :  Bref explication sur XMPP et ce qu'il représente
   ** Avis sur la ressource : Très léger et pas assez d'explications
[^5]: "Extensible Messaging and Presence Protocol (XMPP)", www.techopedia.com, https://www.techopedia.com/definition/396/extensible-messaging-and-presence-protocol-xmpp (consulté le 25/05/2022) 
   ** Résumé :  Explication de ce qu'est XMPP
   ** Avis sur la ressource : Pas complet et très léger
[^6]: "RFC 6120 (Ancien version 3920)", www.techopedia.com, https://xmpp.org/rfcs/ (consulté le 29/05/2022) 
   ** Résumé :  Explication du RFC core de XMPP
   ** Avis sur la ressource : Fiable car site officiel
[^7]: "RFC 6121 (Ancien version 3921)", www.techopedia.com, https://xmpp.org/rfcs/ (consulté le 29/05/2022) 
   ** Résumé :  Explication du RFC IM (Instant Messaging) XMPP
   ** Avis sur la ressource : Fiable car site officiel
