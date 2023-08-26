A.J.jacquie MacLaine /Vinck, University of Duisburg-Essen
layout: default
title: TLS
parent: Réseaux
---
# TLS [^1]

## qu'est ce que c'est?
TLS ou "Transport Layer Security" est un protocole permettant à deux entités de discuter sur Internet de manière sécurisée en encryptant les données transférées. Cette méthode permet d'éviter l'écoute de conversation, les altérations tierces et la création de messages frauduleux. TLS est prévu pour une utilisation qui encapsule des données de couche applicative, par exemple la requête d'une page web.

## étapes d'une transmission

Voici à quoi ressemble une communication tls:
![](https://cdn.discordapp.com/attachments/1081507557023170620/1113845245080252497/image.png)

Voyons étape par étape ce qu'il ce passe

### handshake
#### 1. hello

Avant la transmission des données du payload, le client et le serveur commencent par un échange de "hello". Ces "hello" contiennent une série d'informations qui seront nécessaires plus tard :

- `legacy_version` signale la version de TLS qui sera utilisée.
- `random` est un nombre aléatoire sur 32 octets, soit 256 bits.
- `cipher_suites` est une liste de paires d'algorithme de hachage et de valeur. La plus importante est la clé publique pour l'échange Diffie-Hellman (voir *cryptographie* plus bas)

Donc, dans la situation vue ci-dessus, le client envoie un "clientHello" contenant les versions de TLS qui peuvent être utilisées (c'est le `legacy_version`), le `random`, ainsi que les `cipher_suites` (paires d'algorithme de hachage et de valeur).

Le serveur, en recevant le message, décide de l'algorithme de chiffrement qui sera utilisé et répond avec un `ServerHello` suivant la même logique que le `clientHello`. À partir de ce point, tous les échanges seront cryptés.

#### 2. les extensions

Immédiatement après avoir envoyé le `ServerHello`, le serveur envoie un `EncryptedExtensions` qui contient la liste des extensions que le serveur peut utiliser. Cela inclut, entre autres, le protocole de signature, la validation étendue ou la transparence des certificats.

#### 3. le finished

Après l'envoi de toutes les informations optionnelles, le serveur envoie un `finished` qui déclare la fin du handshake. Ce message contient une clé qui sert à authentifier le handshake.

En recevant le `finished`, le client doit l'utiliser pour vérifier l'intégrité du handshake. Si la moindre chose est incorrecte, la connexion est terminée et devra être relancée.

Une fois validé, le client fait de même et envoie un `finished` au serveur qui suit les mêmes étapes de validation. En cas d'inconsistance, le serveur termine également la connexion.

### transfert de donnée
Une fois le handshake terminé, l'échange de données commence. Pour cela, les données à envoyer sont fragmentées, c'est-à-dire séparées en petites unités appelées "TLS records". Ces records contiennent un payload (une partie des données à transmettre) et un en-tête qui permettra de remettre les records dans l'ordre.

Avant l'envoi, chaque record est encrypté en utilisant l'algorithme de chiffrement décidé lors du handshake. Le record peut alors être envoyé sur le réseau via UDP ou TCP, selon les besoins de l'application. Une fois arrivé, le reseveur peut donc décrypter chaque record et les rassembler.

Une fois décryptées et reconstituées, les données peuvent être transmises à l'application de destination.

### fin de transmission
Il y a plusieurs possibilités de fin de transmission :
#### fin gracieuse
- Initiation par le client : Le client envoie un `close_notify` au serveur, qui répond par un autre `close_notify`. Une fois la réponse reçue, le client considère la connexion comme terminée.
- Initiation par le serveur : Similaire à la fin initiée par le client, mais dans le sens inverse.

#### fin abrupte
La transmission peut se terminer de manière abrupte pour diverses raisons, allant de la terminaison de la transmission TCP sous-jacente à un problème de réseau. L'hôte termine la transmission sans envoyer de `close_notify`. L'effet de cela est que l'autre hôte ne peut pas faire la distinction entre une fin abrupte et une perte de connexion.

## la cryptographie

Maintenant que nous avons vu les aspects de fonctionnement de TLS, il faut aborder l'aspect cryptographique qui est indispensable pour son bon fonctionnement.

- **encryption symétrique** Avec des algorithmes tels que AES, 3DES ou ChaCha20, ils sont utilisés pour les données utilisateurs lors du *transfert de données*. Pour une encryption symétrique, le principe de base est d'utiliser la même clé pour l'encodage et le décodage, cette clé est donc secrète. Elle est générée par l'émetteur (souvent le serveur) et doit être générée avant l'échange, dans le *handshake*.

- L'**encryption asymétrique** est utilisée pour générer la clé symétrique dans le *handshake*. Pour ce système, il faut quatre clés : le serveur et le client génèrent chacun une clé privée et une clé publique. Les clés sont échangées dans la suite de chiffrement (*cipher suite*) des messages `hello`. Une différence est faite dans le fait que la suite de chiffrement du serveur est authentifiée avec un certificat d'autorité (CA).  
À partir d'ici, le client et le serveur peuvent utiliser Diffie-Hellman pour trouver la clé d'encryption symétrique.  
L'encryption asymétrique est également utilisée pour les signatures des messages, ici avec RSA ou ECDSA.

- **Diffie-Hellman**. Sans trop rentrer dans les détails mathématiques, cet algorithme permet à deux communicants de trouver une clé privée commune sans jamais la transmettre directement entre eux. Voici un diagramme grossièrement montrant son fonctionnement avec des couleurs [^2]:
![](https://cdn.discordapp.com/attachments/665314692549312513/1129346683449397348/image.png)  
Notez que le mélange des couleurs est en fait une opération mathématique virtuellement irréversible. Il n'est donc pas possible pour quelqu'un qui intercepte la transmission de retrouver l'une ou l'autre clé privée.

*note: La quantité de calculs à effectuer ici et dans les autres étapes de l'encryption asymétrique est la raison pour laquelle il est nécessaire d'utiliser l'encryption symétrique (beaucoup moins gourmande) pour les données.*

- Les **fonctions de hachage** sont le mécanisme par lequel le serveur et le client peuvent s'assurer de l'intégrité des données (SHA-256 ou SHA-384). Le fonctionnement est très simple : l'expéditeur envoie avec les données le hash de ces même données. Pour vérifier si les données sont intactes, le destinataire calcule également le hachage des données reçues et compare les deux. S'ils ne correspondent pas, cela signifie qu'une erreur s'est glissée soit dans le hachage, soit dans les données. Dans les deux cas, la portion de données sera renvoyée.

## Bibliographie

[^1]:  E. Rescorla, [RFC 8446 - The Transport Layer Security (TLS)](https://www.rfc-editor.org/rfc/rfc8446), August 2018
    
       **Résumé** : RFc décrivant les principes de TLS
       **Avis sur la ressource** : article d'authoritée
[^2]: A.J. Han Vinck [intoduction to public key criptography](https://commons.m.wikimedia.org/wiki/File:Diffie-Hellman_Key_Exchange.svg#mw-jump-to-license)

       **Résumé** : schéma explicatif de Diffie-Hellman avec des couleurs
       **Avis sur la ressource** : suffisant pour une compréhension globale de l'algorithme

