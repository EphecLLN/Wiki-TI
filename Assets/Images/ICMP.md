---
layout: default
title: ICMP
parent: Réseaux
---

# ICMP

## Qu'est ce que le protocole ICMP ?
---

ICMP ou Internet Control Message Protocol est un protocole faisant parti de l'ensemble des protocoles TCP/IP et sert à la transmission de messages de contôle et d'erreur. 

A ce jour, il existe 2 versions : une pour l'Ipv4, décrite dans l'RFC 792 en 1981, et une pour l'Ipv6, décrite dans l'RFC 4443 en 2006.

## Pourquoi utiliser ICMP ?
---

Le but d'ICMP est de signaler et contrôler les problèmes lors d'une transmission. Par exemple, c'est grâce à lui qu'on sait que la destination n'est pas atteignable ou qu'il y a eu une redirection. De ce faite, il doit être implémenter dans chaque module IP.
Il faut également savoir qu'aucun message ICMP n'est envoyé pour répondre à la reception d'une autre message ICMP et ce, afin d'éviter des problèmes de bouclage sans fin.

Pour finir, faite attention, son but n'est pas de rendre le protcole IP (Internet Protocol) fiable. Il ne garanti en rien l'arrivée d'un paquet de données à sa destination et ne signale pas la perte d'un paquet lors de sa transmission. 


## ICMP et le modèle OSI
---

Le protocole ICMP est un protocole de couche 3 sur le modèle OSI mais est traité comme un se situant sur une couche supérieur lors de son encapsulation dans le protocole IP.
<p align="center">
    <img width="380" alt="Modèle OSI" src="../Assets/Images/Modèle_OSI.png">
</p>

## Fonctionnement d'ICMP
---

ICMP utilise un système de types et de codes qui permet d'identifier le type de message et l'état de ce-dis type.

### Les types et leurs codes
---
Les types sont différenciés à l'aide d'un numéro allant de 0 à 255 qui leur est propre et sont accompagnés par un ou plusieurs codes permettant d'identifier le problème/contrôle.

Exemples en IPv4 :
Type | Code | Description
---- | ---- | -----------
0 - Réponse d'écho | 0 | Réponse d'echo 
3 - Destinataire innaccessible | 0 | Réseau innaccessible
3 - Destinataire innaccessible | 1 | Machine innaccessible
3 - Destinataire innaccessible | 2 | Protocole innaccessible
3 - Destinataire innaccessible | 3 | Port innaccessible
8 - Demande d'écho | 0 | Demande d'écho

Il faut également savoir que les numéros de types sont différents suivant la version d'ICMP qu'on utilise. Ainsi, le type 1 en IPv6 n'est pas forcement le même qu'en IPv4.

Si vous voulez plus de détails sur ces numéros et codes, voici la documentation de L'IANA à ce sujet : <a href="http://www.iana.org/assignments/icmpv6-parameters/icmpv6-parameters.xhtml#icmpv6-parameters-2"> IPv6 </a> et <a href="https://www.iana.org/assignments/icmp-parameters/icmp-parameters.xhtml"> IPv4 </a>


### Format d'un paquet ICMP
---

Même si le protocole ICMP se trouve sur la même couche que le protocole IP, il ne dispose pas d'un paquet propre. En effet, il est directement encapsulé dans un paquet IP. De ce faite, ICMP est traité par IP comme étant un protocole de couche supérieur. 

Le format en IPv4 contient :

<p align="center">
    <img alt="Paquet IPv4 avec ICMP" src="../Assets/Images/Paquet_IPv4_ICMP.png">
</p>

- un en-tête IPv4 (en vert). La valeur de _Protocole_ est 1 et celle de _Type de Service_ est 0.
- un en-tête ICMP (en bleu) :
    - Type : le numéro du type de message
    - Code : le code décrivant l'état du type
    - Somme de contrôle : permet de vérifier que les données n'ont pas été corrompu en chemin.

Le format en IPv6 est sembable à celui en IPv4 mais sont en-tête IP est adapté à la version 6.

## Bibliographie 

1. [Qu'est-ce que le protocole ICMP ?](https://www.ionos.fr/digitalguide/serveur/know-how/quest-ce-que-le-protocole-icmp/), Digital Guide IONOS, le 05/03/2019, consulté le 31/05/2022

2. [RFC 792](https://datatracker.ietf.org/doc/html/rfc792), J.Postel, septembre 1981, consulté le 31/05/2022

3. [RFC 4443](https://datatracker.ietf.org/doc/html/rfc4443), A.Conta & S.Deering & M.Gupta, mars 2003, consulté le 31/05/2022

4. [Internet Control Message Protocol](https://fr.wikipedia.org/wiki/Internet_Control_Message_Protocol#cite_note-5), Wikipedia, 27/12/2021, consulté le 31/05/2022

5. [Internet Control Message Protocol V6](https://fr.wikipedia.org/wiki/Internet_Control_Message_Protocol_V6), Wikipedia, 10/10/2017, consulté le 31/05/2022

6. [ICMP](https://www.editions-eni.fr/open/mediabook.aspx?idR=769233a055093dd8a2a414361a469394), Cisco, consulté le 31/05/2022
