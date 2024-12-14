---
layout: default
title: TLS
parent: Sécurité
---

# Transport Layer Security

## Introduction
Transport Layer Security, ou TLS, a pour objectif de sécuriser les échanges effectués sur la couche transport. Il offre une surcouche à TCP, et permet aux protocoles applicatifs de bénéficier de conversations chiffrées de bout en bout <sup>[[1]](#bib1)</sup>. Son cas d'utilisation le plus répandu est HTTPS (Hyper Text Transfer Protocol Secure), permettant la sécurité des opérations effectuées sur le web, mais TLS reste tout de même un standard populaire pour d'autres applications nécessitant un transport sécurisé, comme par exemple l'email, la messagerie, ou encore la téléphonie sur IP (VoIP).

Voici les principales caractéristiques de TLS permettant d'assurer un transport des données de manière sécurisée : 
- Chiffrement : protège la session grâce à une combinaison d'algorithmes symmétriques et asymmétriques, rendant les données inexploitables sans possession des clés cryptographiques.
- Authentification : permet d'assurer que les partis communiquants sont réellement qui ils prétendent être, grâce à des certificats TLS.
- Intégrité : vérifie la validité de chaque paquet grâce à son Message Authentication Code (MAC), permettant de détecter une altération ou perte de données.

La première version de TLS a été initialement publiée en 1999 dans le [RFC 2246](https://datatracker.ietf.org/doc/html/rfc2246), et visait à succéder à SSL (Secure Sockets Layer), dont la dernière version, 3.0, a été dépréciée en 2015 ([RFC 7568](https://datatracker.ietf.org/doc/html/rfc7568)), pour des raisons d'obsolescence (insécurité, capacités limitées).

La classification de TLS dans le modèle OSI est assez ambigüe en raison de ses caractéristiques. En effet, puisqu'il opère au dessus de TCP, il devrait se trouver dans une couche supérieure à la couche 4. Cependant, il transporte de manière fiable des données applicatives, cela pourrait définir un protocole de couche 4. En raison de ses objectifs de chiffrement, TLS est généralement classé dans la couche présentation <sup>[[2]](#bib2) [[3]](#bib3)</sup>, responsable de la traduction des données. Notons cependant que le modèle OSI est plus complexe à appliquer dans ce cas-ci, et qu'il peut ne pas représenter la réalité qui est parfois impossible à généraliser.

## Historique
Au début de l'ère d'Internet, les communications étaient effectuées en clair. De ce fait, des protocoles comme HTTP étaient extrêmement vulnérables, puisque les données étaient potentiellement exploitables par quiconque parvenait à mettre la main dessus. Il a fallu attendre le milieu des années 90 pour disposer de moyens permettant de sécuriser le traffic circulant sur Internet.

### SSL 1.0
En 1994, Netscape Communications s'apprêtait à publier la première version de son protocole Secure Sockets Layer, apportant une couche de sécurité au protocole TCP. Cependant, des vulnérabilités critiques ont rapidement été détectées, et cette version 1.0 n'a jamais été publiée.

> "The actual history of SSL was that SSL 1.0 was so bad that Alan Schiffman and myself broke it in ten minutes when Marc Andressen presented it at the MIT meeting." [Phillip Hallam-Baker](https://en.wikipedia.org/wiki/Phillip_Hallam-Baker) <sup>[[4]](#bib4)</sup>

### SSL 2.0
En 1995, le protocole SSL a enfin été rendu disponible au public avec la version 2.0, et a servi de base à l'arrivée de la sécurité sur le web, avec la sortie du protocole HTTPS. Malheureusement, comme pour la version 1.0, des failles de sécurités ont rapidement été détectées dans SSLv2, comme par exemple de faibles algorithmes de chiffrement, ou bien une grande vulnérabilité aux attaques de type Man-In-The-Middle (interception du flux de données, permettant de prendre possession des clés, et donc de déchiffrer l'information). Ces raisons ont mené à sa dépréciation en 2011 ([RFC 6176](https://www.rfc-editor.org/rfc/rfc6176)).

### SSL 3.0
Les défauts des précédentes versions de SSL ont nécessité une reconception intégrale du protocole. C'est en 1996 que la version 3.0 voit le jour. Cette dernière, plus robuste, a été largement employée pendant des années. Certaines failles ont été détectées au fil du temps ([BEAST](https://learn.microsoft.com/en-us/archive/blogs/kaushal/taming-the-beast-browser-exploit-against-ssltls), [BREACH](https://www.breachattack.com/)), et c'est en 2014 que la découverte de l'attaque POODLE ([CVE 2014-3566](https://nvd.nist.gov/vuln/detail/CVE-2014-3566)) a officiellement mis un terme à la pertinence de SSLv3, qui a été déprécié en 2015 ([RFC 7568](https://www.ietf.org/rfc/rfc7568)).

### TLS 1.0
En janvier 1999, l'IETF (Internet Engineering Task Force) a inauguré la première version de TLS ([RFC 2246](https://datatracker.ietf.org/doc/html/rfc2246)), visant à succéder à SSL. Bien que TLS se base sur les spécifications du protocole SSL de Netscape, et les deux protocoles étant globalement assez similaire, l'apport principal de cette refonte intégrale est le passage vers un standard beaucoup plus libre. Il a été déprécié en 2021 ([RFC 8996](https://datatracker.ietf.org/doc/html/rfc8996))

### TLS 1.1
Cette révision ([RFC 4346](https://datatracker.ietf.org/doc/html/rfc4346)) de TLS 1.0, datant de 2006, apporte des améliorations de sécurité et d'autres rectifications mineures. La vulnérabilité principale à l'origine de ce patch concernait le CBC (Cipher Block Chaining), mécanisme de défense dont la valeur pouvait être prédite, autorisant donc des attaques. TLS 1.1 a également été déprécié en 2021 par le [RFC 8996](https://datatracker.ietf.org/doc/html/rfc8996).

### TLS 1.2
En 2008, la version 1.2 de TLS a été publiée ([RFC 5246](https://datatracker.ietf.org/doc/html/rfc5246)). Cette dernière visait à améliorer la complexité cryptographique du protocole, notamment par une migration de certaines étapes de chiffrement de MD5/SHA-1 vers d'autres alternatives beaucoup plus puissants comme SHA-256. La négociation des algorithmes de chiffrement a également été améliorée.

Depuis sa publication initiale, TLS 1.2 a subi des améliorations visant à conserver sa pertinence dans le temps ([RFC 6176](https://datatracker.ietf.org/doc/html/rfc6176), [RFC 8446](https://datatracker.ietf.org/doc/html/rfc8446#section-1.3)). TLS 1.2 est aujourd'hui la plus ancienne version encore supportée (99.9% en date du 02 mai 2024 <sup>[[5]](#bib5)</sup>), et sa dépréciation n'a pas encore été prévue.

### TLS 1.3
En août 2018, le [RFC 8446](https://datatracker.ietf.org/doc/html/rfc8446) annonce l'arrivée de TLS 1.3, avec un grand nombre d'améliorations, notamment : 
- La dépréciation d'un bon nombre d'algorithmes de chiffrement (MD5, SHA-224, ...) et fonctionnalités (compression, renégociation) devenus obsolètes
- La séparation des mécanismes d'authentification et d'échange de clés au niveau des cipher suites (ensemble prédéfini d'algorithmes de chiffrement <sup>[[6]](#bib6)</sup>)
- L'instauration du mode [0-RTT](https://techdocs.akamai.com/property-mgr/docs/early-data-0rtt) (Zero Round-Trip Time) permettant au client récemment authentifié d'envoyer des paquets de données pendant l'initialisation de la session TLS ([handshake](#tls-handshake)). Cette addition élimine la nécessité d'attendre la réponse du serveur avant de commencer l'émission de données, mais le protocole devient alors sensible à de potentielles attaques Replay (duplication de paquets).

TLS 1.3 est aujourd'hui la version la plus récente et robuste du protocole. Cette dernière est supportée à 70.1% en date du 02 mai 2024 <sup>[[5]](#bib5)</sup>. Aujourd'hui, elle ne supporte que des algorithmes dont aucune vulnérabilité n'est connue. Malheureusement, il n'est pas à écarter l'option que des failles soient découvertes, et que TLS 1.3 devienne obsolète.

## Fonctionnement
L'échange TLS est caractérisé par deux phases : 
- La poignée de main/handshake, servant à initier la session, lors de laquelle le serveur procure son certificat permettant de vérifier son identité
- Le transfert de données chiffrées sous forme de Records, et dont l'intégrité et l'authenticité sont assurées par un Message Authentication Code (MAC)

### Certificats
Pour parvenir à communiquer via TLS, un serveur a besoin d'une preuve d'identité, permettant au client de s'assurer que son interlocuteur est réellement qui il prétend être. Pour cela, TLS emploie des certificats (SSL en utilisait également, d'où la persistence de l'expression SSL certificate).

Les certificats sont générés par des autorités tierces (Certificate Authorities ou CA) telles que [Let's Encrypt](https://letsencrypt.org/) ou [GlobalSign](https://www.globalsign.com), et octroyés après validation de l'authenticité du demandant. Un certificat contient des champs tels que le nom de domaine, la CA correspondante et sa signature, et une clé publique <sup>[[7]](#bib7)</sup> .

Bien que principalement utilisés par les serveurs, les certificats TLS existent également pour les clients. Leur objectif d'authentification reste le même, et ils peuvent être utilisés notamment lorsqu'un serveur souhaite restreindre l'accès à des clients vérifiés, de manière plus sécurisée qu'un simple mot de passe.

### <a id="tls-handshake">Handshake</a>

L'initialisation de la session TLS est représentée par une four-way handhsake (poignée de main en quatre étapes). Elle a pour objectifs de négocier les algorithmes de chiffrement utilisés lors de la session, de valider l'authenticité du serveur (grâce à son certificat), et d'assurer l'échange des clés cryptographiques qui seront utilisées lors du transfert de données <sup>[[8]](#bib8) [[9]](#bib9)</sup> . Voici les échanges effectués lors cette four-way handshake : 
1. ClientHello : Le client débute la conversation par un message ClientHello, contenant des informations telles que sa version de TLS, un nombre aléatoire qui servira à générer les clés de session et une cipher suite (liste ordonnée d'algorithmes supportés pour les différentes étapes de la conversation).
2. ServerHello / Certificate : Le serveur analyse le ClientHello et choisit la plus récente version de TLS supportée, une cipher suite correspondant à la proposition du client, et génère à son tour un nombre aléatoire. Il envoie également son certificat pour prouver son identité, ainsi qu'un ServerKeyExchange (si demandé par l'algorithme sélectionné pour l'échange de clés).
3. ClientKeyExchange / Finished : Le client envoie un message permettant l'échange de clés et un Finished indiquant qu'il est prêt à échanger de l'information chiffrée sous forme de Records.
4. Finished : Le serveur répond à son tour avec un Finished. Tout échange ultérieur nécessitera les clés cryptographiques pour être déchiffré.

Notons que les étapes de la poignée de main son sujettes à plusieurs variantes. Par exemple, certains environnements nécessitent aussi une authentification du client (via son propre certificat) , et/ou le premier record pourrait être réceptionné avant le ServerHello si la mécanique 0-RTT est activée.

Comme expliqué ci-dessus, plusieurs clés interviennent lors de la four-way handshake. Voici de brèves explications pour clarifier la terminologie employée dans le cadre de TLS : 
- Clé publique : Il s'agit de la clé contenue dans le certificat du serveur, et est accessible à tout le monde.
- Clé privée : Seul le serveur possède cette clé qui lui a été conférée lors de la création de son certificat. Il l'utilise pour déchiffrer les messages chiffrés par sa clé publique. Elle n'a d'utilité que lors de la poignée de main qui permet de générer d'autres clés.
- Pre-Master Secret : Cette information est calculée grâce à une combinaison du ServerKeyExchange (si l'algorithme le demande), du ClientKeyExchange et de la clé publique. 
- Master Secret : Cette clé est dérivée sur base du Pre-Master-Secret grâce à une PRF (Pseudo Random Function). La dérivation a pour objectif d'améliorer la sécurité cryptographique des clés, mais également d'assurer l'unicité du Master Secret pour chaque session.
- Clés de session : Le serveur et le client partagent une paire de clés symmétriques (deux clés identiques), qu'ils utilisent à la fois pour chiffrer et déchiffrer les Records. Elles sont générées par dérivation du Master Secret par une PRF, utilisant les nombres aléatoires partagés par le client et le serveur pour assurer des clés uniques en cas de Master Secret identique (reprise de connexion).

### Message Authentication code (MAC)
Une fois que la session est initiée, le client et le serveur peuvent à présent envoyer et recevoir des données. Pour vérifier la validité de l'information, chaque Record sera accompagné d'un code d'authentification ou MAC. Lorsque le récepteur reçoit le message, il aura comme tâche de calculer son MAC en employant sa clé de session, afin d'assurer qu'il correspond à celui inclus dans le Record.

Les deux partis doivent se mettre d'accord sur la méthode employée pour calculer les MAC. En effet, l'ordre des opérations à effectuer diffère pour les algorithmes MAC-then-Encrypt (d'abord calculer le MAC) et Encrypt-then-MAC (d'abord chiffrer le message). Notons qu'avec MAC-then-Encrypt, si le MAC n'est pas valide (pour quelconque raison), le récepteur aura effectué deux opérations avant de se débarasser du message. Encrypt-then-MAC propose une solution permettant d'écarter directement les Records altérés <sup>[[8]](#bib8)</sup> .

## Alternatives
TLS est naturellement un standard très répandu dans le domaine des échanges applicatifs chiffrés. Cependant, il existe plusieurs alternatives permettant de répondre à des besoins similaires ou légèrement différents. En voici une liste non exhaustive : 
- DTLS, Datagram Transport Layer Security (initialement décrit dans [RFC 4347](https://www.rfc-editor.org/rfc/rfc4347)) : tourne sur UDP, et offre donc un support pour différents protocoles applicatifs tels que SIP (Session Initiation Protocol) qui est utilisé notamment pour la VoIP et la communication vidéo.
- QUIC : protocole développé par Google pour permettre à HTTP de tourner par dessus UDP, utilisant un chiffrement similaire à TLS, et permettant de limiter la quantité d'information nécessaire à établir la connexion (assez lourde avec TCP).
- SSH, Secure Shell (initialement décrit dans [RFC 4253](https://datatracker.ietf.org/doc/html/rfc4253)) : protocole offrant un canal de communication sécurisé dans un environnement distant non sécurisé, dont les cas d'utilisation sont assez distincts de TLS, mais ayant recours à des méthodes très similaires pour atteindre ses objectifs.

Ces protocoles, mais également bien d'autres plus ou moins répandus, permettent d'assurer différents types de communication de manière sécurisée. Notons qu'aucun n'a réellement pour objectif de remplacer TLS, qui dispose de ses cas d'usage bien définis, mais plutôt d'étendre les possibilités pour couvrir un maximum de cas de figure, et ainsi procurer des options sécurisées quels que soient les besoins de l'application.

## Bibliographie
- <a id="bib1">[TLS Basics](https://www.internetsociety.org/deploy360/tls/basics/)</a> <sup>[1]</sup>, Internet Society, consulté le 10/12/2024
- <a id="bib2">[A high level view of TLS/SSL](https://www.ibm.com/docs/en/informix-servers/15.0.0?topic=tls-high-level-view-tlsssl)</a> <sup>[2]</sup>, IBM, dernière modification le 19/11/2024, consulté le 10/12/2024
- <a id="bib3">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)</a> <sup>[3]</sup>, Wikipedia, dernière modification le 12/12/2024, consulté le 12/12/2024
- <a id="bib4">[Crypto Standards v.s. Engineering habits](https://www.metzdowd.com/pipermail/cryptography/2013-October/018041.html)</a> <sup>[4]</sup>, Phillip Hallam-Baker, 04/10/2010, consulté le 11/12/2024
- <a id="bib5">[SSL Pulse](https://www.ssllabs.com/ssl-pulse/)</a> <sup>[5]</sup>, Qualys Inc, 02/05/2024, consulté le 11/12/2024
- <a id="bib6">[What are cipher suites?](https://www.networking4all.com/en/support/faq/what-are-cipher-suites)</a> <sup>[6]</sup>, Networking4all, consulté le 11/12/2024
- <a id="bib7">[What is an SSL/TLS Certificate?](https://aws.amazon.com/what-is/ssl-certificate/)</a> <sup>[7]</sup>, Amazon Web Services Inc, consulté le 11/12/2024
- <a id="bib8">[Computer Networking : Principles, Protocols and Practice](https://beta.computer-networking.info/syllabus/default/protocols/tls.html)</a> <sup>[8]</sup>, Olivier Bonaventure, dernière modification le 12/12/2024, consulté le 12/12/2024
- <a id="bib9">[What happens during a TLS handshake?](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)</a> <sup>[9]</sup>, Cloudflare Inc, consulté le 12/12/2024
- [SSL and TLS Protocols](https://wiki.openssl.org/index.php/SSL_and_TLS_Protocols), OpenSSL, dernière modification le 27/06/2017, consulté le 12/12/2024
- [The Illustrated TLS Connection](https://tls12.xargs.org/), consulté le 12/12/2024
- [The POODLE Attack and the End of SSL 3.0](https://blog.mozilla.org/security/2014/10/14/the-poodle-attack-and-the-end-of-ssl-3-0/), Richard Barnes (Mozilla Corporation), dernière modification le 14/10/2014, consulté le 11/12/2024
- [What is TLS (Transport Layer Security)?](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/), Cloudflare Inc, consulté le 11/12/2024
