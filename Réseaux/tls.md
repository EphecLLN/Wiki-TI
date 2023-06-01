---
layout: default
title: TLS
parent: Réseaux
---
# TLS [^1]

## qu'est ce que c'est?
TLS ou 'Transport Layer Security' est un protocol permettant a deux entité de discuter sur internet de manière sécurisée en encryptant les donnée transférée. cette méthode permet d'éviter l'écoute de conversation, les alteration tierce et la création de message frauduleux. tls est prévu pour une utilisation qui encapsule des donnée de couche applicative. par exemple la requête d'une page web. 

## étapes d'une transmission

voici a quoi ressemble une communication tls:
![](https://cdn.discordapp.com/attachments/1081507557023170620/1113845245080252497/image.png)

voyons étape par étape ce qu'il ce passe

### handshake
#### 1. hello
avant la transmission de donnée du payload, le client et le serveur commencent par un échange de hello. ces hello contiennent une série d’informations qui sera nécessaire plus tard:
- `legacy_version` signal la version de tls qui sera utilisée
- `random` est un nombre aléatoire sur 32 byte soit 256 bit.
- `cipher_suites` est une liste de pair algorithme de hash et valeur. (c'est de la crypto de haut niveau nous ne rentreront pas dans les détails ici)

donc dans la situation vue au dessus: le client envoi un `clientHello` contenant les versions de tls qui peuvent être utilisée, c'est le `legacy_version`, le `random` et le `cipher_suites`.

le serveur, en recevant le message décide de l'algorithme de chiffrement qui sera utilisé et répond avec un `ServerHello` suivant la même logique que le `clientHello`.
a partir de ce point tout les échanges seront crypté.

#### 2. les extensions
immédiatement après avoir envoyé le `serverHello`, le serveur envoi un `EncryptedExtensions` qui contiens la liste des extension que le serveur peut utiliser. cela antre autre, le protocole de signature, la validation étendue ou la transparence des certificats.

#### 3. le finished
après l'envois de toutes les informations optionnelles, le serveur envoie un `finished` qui déclare la fin du handshake. ce message contient une clef qui sert a authentifier le handshake.

en recevant le `finished` le client doit l'utiliser pour vérifier l’intégrité du handshake. si la moindre chose est incorrecte, la connection est terminée et devra être relancée.

une fois validé, le client fait de même et envoie un `finished` au serveur qui suit les même étape de validation. en cas d'inconsistance, il termine également la connection.

### transfert de donnée
une fois le handshake terminé, l'échange de donnée commence. pour cela les donnée a envoyé sont fragmentée, c'est a dire séparée en petites unités appelée 'TLS records'. ces records contiennent un payload (une partie des donnée a transmettre) et un header qui permettra de remette les records dans l'ordre.

Avant l'envois, chaque record est encrypté en utilisant l’algorithme de chiffrement décidé lors du handshake. le record peut alors être envoyé sur le réseau par UDP ou TCP selon les besoin le l'application. une fois arrivé, le serveur peut donc décrypter chaque records et les rassembler.

une fois décrypter te reconstituée, les donnée peuvent être transmise a l’application de destination.

### fin de transmission
il y a plusieurs possibilité de fin de transmission:
#### fin gracieuse
- initié par le client: le client envoie un `close_notify` au serveur qui répond par un autre `close_notify`. une fois la réponse ressue, le client considère la connection comme terminée.  
- initié par le serveur: idem mais dans le sans inverse.

#### fin abrupte
pour un multitude de raisons allant de la terminaison de la transmission tcp sous-jacente a un problème de réseau. l'hôte termine la transmission sans envoyer de `close_notify`. l'effet est que l'autre hôte ne peut pas faire la distinction entre la fin abrupte et une perte de connection.


## Bibliographie

[^1]:  E. Rescorla, [RFC 8446 - The Transport Layer Security (TLS)](https://www.rfc-editor.org/rfc/rfc8446), August 2018
    
       **Résumé** : 
       **Avis sur la ressource** : 

