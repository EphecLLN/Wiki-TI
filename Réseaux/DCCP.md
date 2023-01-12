---
layout: default
title: DCCP
parent: Réseaux
---
# DCCP (Datagram Congestion Control Protocol)
décrit par [RFC4340](https://www.rfc-editor.org/rfc/rfc4340), ce protocole est prévu comme une alternative aux utilisations moderne de UDP qui posent parfois des problèmes de congestion. Voici une introduction à son fonctionnement.

## Pourquois DCCP?

en 2006, [RFC4336](https://datatracker.ietf.org/doc/html/rfc4336) soulève le problème causé par la popularisation massive des services de streaming et des jeux MMO sur la congestion d'internet. Ces services qui font primer la vitesse d’arrivée d'une transmission sur la "qualité" de celle-ci se sont naturellement tourné vers UDP pour éviter le mécanisme de renvois de segment de TCP qui leurs est inutile.
Malheureusement cette augmentation n'est pas sans impact car UDP ne possède pas de contrôle de congestion.

Le problème avais déjà été soulevé par [RFC2914](https://datatracker.ietf.org/doc/html/rfc2914) en 2000 et a nouveau par [RFC3714](https://datatracker.ietf.org/doc/html/rfc3714) en 2004.

DCCP vise à apporter une solution à ce problème en fournissant un transfert rapide et non fiable de datagrammes et ce avec un contrôle de congestion.

## Format de paquets

![DCCP-packet-structure](https://cdn.discordapp.com/attachments/1031999772694954084/1063051209008754818/image.png)

DCCP possède deux type d'entête générique, la différence est dans le nombre de bits attribué au numéro de séquence.

Dans le premier le bit X est à 1. ce qui indique que l'on utilise les numéro de séquence étendu (48 bits). De plus, les bits marqué *reserve* doivent être mis a 0 et ignoré par le receveur.
![DCCP-header-X=1](https://cdn.discordapp.com/attachments/1031999772694954084/1063048311877161010/image.png)

Le second utilise seulement 24 bits pour le numéro de séquence. Désormais, les bits qui était marqué *reserve* sont utilisé par les MSB du numéro de séquence.
![DCCP-header-X=0](https://cdn.discordapp.com/attachments/1031999772694954084/1063048444534591538/image.png)

- **Source Port**: identifie le port source du paquet en couche 4
- **Destination Port**: identifie le port de destination du paquet en couche 4
- **Data Offset**: donne le décalage entre le début du packet DCCP et le début des donnée applicative, par pas de 32 bits.
- **CCVal**: dicte le contrôle de congestion utilisé (2 ou 3).
- **Checksum Coverage (CsCov)**: spécifie quelle partie du paquet sont couvert par le *CHECKSUM*. Les options et l'entête sont toujours couverts.
- **Checksum**: Checksum du paquet, dépend de *CsCov*.
- **Reserved**: Bits mis à 0 et ignoré par le receveur
- **Type**: spécifie le type de packet (détails plus bas)
- **Sequance Number**: identifie de manière unique le paquet parmi tout ceux envoyé durant cette connexion. Il est incrémenté à chaque paquet envoyé à l’exception des ACK.

Si présent, l’entête générique est directement suivie par le *ACK Number*.
comme il y a deux longueurs de numéro de séquences, il y a deux variantes de ACK:

- pour le X à 1

![32-bit-wide](https://cdn.discordapp.com/attachments/1031999772694954084/1063063517357416478/image.png)
![ACK-X=1](https://cdn.discordapp.com/attachments/1031999772694954084/1063062857924739102/image.png)

- pour le X à 0

![ACK-X=0](https://cdn.discordapp.com/attachments/1031999772694954084/1063062923020353630/image.png)

## Types de paquets

seul les *type* 0 à 9 sont utilisé les autres sont *réservé*.
|code|nom  | Ack? | Data? | X? |
|-|-        | - | - | - | - |
|0|Request  |Non|Oui     |1
|1|Response |Oui|Oui     |1
|2|Data     |Non|Oui     |0/1
|3|Ack      |Oui|Non*[^1]|0/1
|4|Data Ack |Oui|Oui     |0/1
|5|Close Req|Oui|Non*[^1]|1
|6|Close    |Oui|Non*[^1]|1
|7|Reset    |Oui|Oui*[^2]|1
|8|Sync     |Oui|Non*[^1]|1
|9|Sync Ack |Oui|Non*[^1]|1
|10-15|réservé|/|/|/|
[^1]: le champ data peut exister mais doit être ignoré
[^2]: contient le texte de l'erreur qui pousse le *reset*.

## Handshake

DCCP utilise le "three way handshake" pour l'établissement de la connexion:

|établissement|détail|
|-|-|
|Request -->|le client demande la connexion
|<-- Responce|le serveur accepte
|Ack ou Data Ack-->|l'échange commence

pour la fin de connexion, plusieurs possibilités:

|fermeture 1|détail|
|-|-|
|<-- Close Req|le serveur demande la fermeture de connexion, mais ne veut pas faire le *TIMEWAIT*
|Close -->| le client accepte de fermer la connexion
|<-- Reset|le serveur ferme la connexion
|TIMEWAIT|le **CLIENT** fait le *TIMEWAIT*

|fermeture 2|détail|
|-|-|
|Close -->| le client veut fermer la connexion
|<-- Reset|le serveur ferme la connexion
|TIMEWAIT|le **CLIENT** fait le *TIMEWAIT*

|fermeture 3|détail|
|-|-|
|<-- Close| le serveur veut fermer la connexion
|Reset -->|le client ferme la connexion
|TIMEWAIT|le **SERVEUR** fait le *TIMEWAIT*

**Time Wait**
a la fin de toutes conversation, l'un des deux interlocuteurs fait un TIMEWAIT. c'est a dire, qu'il reste dans cet état durant ~4 min pour s'assurer qu'aucun paquets qui resterait sur le réseau ne puissent interférer avec une nouvelle connexion.

## Options

DCCP décrit un grand nombre d'options listée en détails [ici](https://www.rfc-editor.org/rfc/rfc4340#section-13) et [ici](https://www.rfc-editor.org/rfc/rfc4340#section-6.1). voici la version courte:

![DCCP-options](https://cdn.discordapp.com/attachments/1031999772694954084/1063072280604835871/image.png)

Ces options permettent une grande adaptabilité de DCCP. Elle peuvent, si besoin, être re-négocier au milieu d'une conversation. Grace au champs d'options 32 à 35 qui permettent de négocier de nouvelle valeurs d'options tout en continuant la transmission de donnée.

Certaine règles sont utilisée lors de la remise dans l'ordre des paquets pour s'assurer que les deux interlocuteurs utilisent les options les plus à jours.

## Perte et Retransmission

Lors de la négociation des options, un paquet perdu doit être "retransmit". Cependant comme il n'y a pas de retransmission de paquet comme le ferait TCP. un nouveau paquet sera envoyer, avec un nouveau numéro de séquence.

## Ordre des Paquets

Comme l'un des objectifs de DCCP est de transmettre les informations les plus à jour possible, les numéros de séquence sont utilisé pour trier les paquets dont les informations ne sont plus d'actualité.
*par exemple: un client d'une plateforme de streaming, peut ignorer toutes les "frames" d'image qui ne sont pas dans l'ordre pour toujours les afficher dans l'ordre a l'écran.*

## Service Code

DCCP offre une identification des protocoles d'applications que les paquets vont utiliser. Il s’agit d'un code à **32 bits** qui suit l'entête lors de l'établissement de la connexion. Cela permet à un pare-feu d'identifier les protocole applicatif utilisé sans pour autant que ceux si utilisent des ports "standard".

## Contrôle de la congestion

DCCP offre deux algorithme pour cela:

- TCP-like Congestion Control. Qui comme TCP utilise AIMD[^3] avec "slow start". Ce qui crée les dents de scie caractéristique de TCP sur le graphique de la taille de fenêtre. Voir [RFC2581](https://www.rfc-editor.org/rfc/rfc2581) pour plus de détails.
- TCP-Friendly Rate Control, utilise, entre autre, le RTT et le délais entre les pertes pour déterminer la meilleure taille de fenêtre. Le but est de s'assurer que la bande passante reste le plus stable possible, tout en évitant la congestion. Cette méthode est idéale pour les applications sensibles à la bande passante, tels que le streaming vidéo.

[^3]: A.I.M.D. -> Additive Increase Multiplicative Decrease

## Diagramme d'états
![STATE-diagram-DCCP](https://cdn.discordapp.com/attachments/1031999772694954084/1063088175712571412/image.png)*à titre indicatif

## Conclusion
DCCP est un protocole de transport qui a pour but de faciliter et standardiser les transmissions rapide et non fiable de datagrammes. Il est principalement utilisé par des services de streaming vidéo et des jeux en lignes. Son principe de fonctionnement est basé sur des paquets de donnée ordonné et sans renvoi des paquets perdu. DCCP utilise également un établissement de connexion similaire à TCP.

## Bibliographie

* [RFC4336](https://datatracker.ietf.org/doc/html/rfc4336), S. Floyd, M. Handley, E. Kohler, Mars 2006, consulté le 12/01/23
  - Résumé : contient toute la problématique et le contexte qui pousse la la création de DCCP.
  - Avis sur la ressource : c'est une source primaire, elle est incontestable

* [RFC4340](https://www.rfc-editor.org/rfc/rfc4340), E. Kohler, M. Handley, S. Floyd, Mars 2006, consulté le 12/01/23
  - Résumé : contient la feuille de spécifications pour DCCP 
  - Avis sur la ressource : c'est une source primaire, elle est incontestable

* [RFC4342](https://www.rfc-editor.org/rfc/rfc4342), S. Floyd, E. Kohler, J. Padhye, Mars 2006, consulté le 12/01/23
