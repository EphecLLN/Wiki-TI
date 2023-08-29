---
layout: default
title: S-MIME
parent: Sécurité
---
# S-MIME

## Introduction

Le mécanisme S-MIME (ou S/MIME) est une abréviation de l'anglais "Secure/Multipurpose Internet Mail Extensions" et est une norme de cryptographie asymétrique utilisée afin d'assurer l'intégrité, l'authentification, la non-répudiation et la confidentialité des e-mails envoyées. <br>
La 1ère version de S/MIME a été développée en 1995, époque où il n'y avait pas une mais plusieurs normes pour la sécurisation des messages. Ce n'est que 3 ans plus tard que la version 2 de S/MIME est sortie et a amené des changements. En effet, cette version a été soumise à l'IETF dans le but de devenir un standard internet. Avec cette étape, S/MIME devient le principal candidat pour une norme de sécurité des messages. Deux demandes de commentaires (RFC) de l’IETF constituent la version 2 de S/MIME : RFC 2311 (https://www.ietf.org/rfc/rfc2311.txt), qui a établi la norme pour les messages, et RFC 2312 (https://www.ietf.org/rfc/rfc2312.txt), qui a établi la norme pour le traitement des certificats.
En 1999, la version 3 de S/MIME a été proposée par l’IETF dans le but d'améliorer S/MIME. La RFC 2632 (https://www.ietf.org/rfc/rfc2632.txt) s’appuie sur le travail de la RFC 2311 pour spécifier les normes pour les messages S/MIME et la RFC 2633 (https://www.ietf.org/rfc/rfc2633.txt) a amélioré la spécification de la gestion des certificats de RFC 2312. La RFC 2634 (https://www.ietf.org/rfc/rfc2634.txt) a étendu les capacités globales en ajoutant des services supplémentaires à S/MIME, tels que les reçus sécurisés, le triple emballage et les étiquettes de sécurité. [1] [2] [3] 

## Fonctionnement

S/MIME fournit, entre autres, deux services qui sont la base de la sécurité des messages.<br>
* signature digitale
* chiffrement de messages

### La signature digitale

Une signature digitale fournit la même chose qu'une signature normale :
* authentification
    Une signature permet de valider son identité aux autres. Comme il n'y a pas d’authentification dans les e-mails SMTP, il est utile de pouvoir fournir une preuve au destinataire afin quil puisse confirmer l'identité de l'expéditeur. 

* non-répudiation
    L’unicité d’une signature empêche le propriétaire de la signature de renier la signature. Cette capacité est donc appelé non-répudiation et permet de rendre juridiquement contraignant les documents signés. Ainsi, les 2 parties s'échangeant des e-mails savent que le contenu de leurs messages peuvent être retenu contre eux en justice et qu'ils ne pourront pas désavouer leurs propos.

* intégrité des données
    Contrairement au document papier qui peut être modifié après sa signature, un document signé par une signature digitale ne peut pas être secrètement modifié car cela rendrait la signature invalide. Le destinataire peut donc être certain que le message n'a pas été modifié lors de sa transmission.

La signature digitale d'un e-mail se fait en 5 étapes.
1)Le message est saisi.<br>
2)Les informations identifiant de manière unique l’expéditeur sont récupérées.<br>
3)L’opération de signature est effectuée sur le message en utilisant les informations uniques de l’expéditeur pour produire une signature numérique.<br>
4)La signature numérique est jointe au message.<br>
5)Envoi du message.<br>

Parce que cette opération nécessite des informations uniques de l’expéditeur, les signatures numériques fournissent une authentification et une non-répudiation. Cette information unique peut prouver que le message ne pouvait provenir que de l’expéditeur.

L'étape inverse, la vérification de la signature numérique d’un message électronique, nécessite un peu plus d'étapes.
7)Reception du message.<br>
6)La signature numérique est extraite du message.<br>
5)Le message est récupéré.<br>
4)Les informations identifiant l’expéditeur sont récupérées.<br>
3)L’opération de signature est effectuée sur le message.<br>
2)La signature numérique incluse avec le message est comparée à la signature numérique produite à la réception.<br>
1)Si les signatures numériques correspondent, le message est valide.<br>

Grâce au système de cryptographie à clé publique, la vérification des informations uniques de l’expéditeur par le destinataire est possible sans réellement connaître ces informations.[3]


### Le chiffrement des messages

Le chiffrement des messages fournit une solution contre l'espionnage de ces messages. Si il n'est pas chiffré, un e-mail est lisible par quiconque l'observe pendant son envoie ou son stockage. Le chiffrement est un moyen de modifier l’information afin qu’elle ne puisse pas être lue ou comprise tant qu’elle n’est pas changée en une forme lisible et compréhensible. Cela fournit deux services de sécurité spécifiques :
* confidentialité
    Seul le destinataire prévu peut voir le contenu réel, et le contenu reste confidentiel car il ne peut pas être reconnu par quiconque qui pourrait recevoir ou visualiser le message. Le chiffrement assure la confidentialité pendant le transport et le stockage du message.

* intégrité des données
    Comme pour les signatures numériques, le chiffrement des messages fournit des services d’intégrité des données en raison des opérations spécifiques qui rendent le chiffrement possible.

Le chiffrement des messages rend le texte d’un message illisible en effectuant une opération de chiffrement sur celui-ci lorsqu’il est envoyé. Lorsque le message est reçu, le texte est rendu lisible à nouveau en effectuant une opération de déchiffrement lorsque le message est lu. 5 étapes sont discernables.

1)Le message est saisi.<br>
2)Les informations identifiant de manière unique le destinataire sont récupérées.<br>
3)Le mécanisme de cryptage est effectuée sur le message en utilisant les informations du destinataire pour produire un message crypté.<br>
4)Le message chiffré remplace le texte du message.<br>
5)Envoi du message.<br>

Vu que cette opération nécessite des informations uniques sur le destinataire, la confidentialité est assurée. Seul le destinataire prévu dispose des informations pour effectuer l’opération de déchiffrement. Cela garantit que seul le destinataire prévu peut voir le message, car les informations uniques du destinataire doivent être fournies avant de visualiser le message non chiffré.

L'étape inverse, déchiffrer un message électronique, a tout autant d'étapes.
5)Reception du message.<br>
4)Le message chiffré est récupéré.<br>
3)Les informations identifiant de manière unique le destinataire sont récupérées.<br>
2)L’opération de déchiffrement est effectuée sur le message chiffré en utilisant les informations uniques du destinataire pour produire un message non chiffré.<br>
1)Le message non chiffré est renvoyé au destinataire.<br>[3]

### La fusion des 2

Voici comment se déroule la signature digitale et le chiffrement d'un e-mail qui va être envoyé :<br>
Le message est saisi. --> Les informations identifiant de manière unique l’expéditeur sont récupérées. -->
Les informations identifiant de manière unique le destinataire sont récupérées. --> L’opération de signature 
est effectuée sur un message en utilisant les informations uniques de l’expéditeur pour produire une signature 
numérique. --> La signature numérique est jointe au message. --> L’opération de cryptage est effectuée sur le 
message en utilisant les informations du destinataire pour produire un message crypté. --> Le message original 
est remplacé par un message chiffré. --> Le message est envoyé. <br>

Et ci-dessous se trouve le déroulement du processus opposé : <br>
Le message est reçu. --> Le message chiffré est récupéré. --> Les informations identifiant de manière unique
le destinataire sont récupérées. --> L’opération de déchiffrement est effectuée sur le message chiffré en 
utilisant les informations uniques du destinataire pour produire un message non chiffré. --> Le message non 
chiffré est renvoyé. --> Le message non chiffré est renvoyé au destinataire. --> La signature numérique est 
extraite du message non chiffré. --> Les informations identifiant l’expéditeur sont récupérées. --> L’opération 
de signature est effectuée sur le message non chiffré en utilisant les informations de l’expéditeur pour 
produire une signature numérique. --> La signature numérique incluse avec le message est comparée à la signature 
numérique produite à la réception. --> Si les signatures numériques correspondent, le message est valide. [3]

## Etat actuel

Beaucoup de grandes entreprises, telles que Google, Facebook et AOL ont déjà prise conscience de l'importance du chiffrement des e-mails depuis quelques années. Et bien que S/MIME ne soit pas utilisé partout, on peut par exemple le trouver dans les version Outlook 2010 ou ultérieures. 
Il est recommandé pour toutes entreprises d'avoir un système de chiffrement d'e-mails, même en interne, pour se prémunir contre le nombre croissant de phishing ou d'usurpation d'identité.<br> [2] [4]

S/MIME est maintenant dans sa version 3.2 avec des RFC plus récentes : 
RFC 5750 : Gestion des certificats (https://datatracker.ietf.org/doc/html/rfc5750)
RFC 5751 : Spécification du message (https://datatracker.ietf.org/doc/html/rfc5751)
[1]

## Bibliographie

[1] https://fr.wikipedia.org/wiki/S/MIME#cite_note-1, wikipédia, consulté le 27/08/23, date article : 21 juin 2018
[2] https://www.globalsign.com/fr/blog/qu-est-ce-que-le-s-mime, GlobalSign Blog, consulté le 27/08/23, date article : 3 mars 2017
[3] https://learn.microsoft.com/fr-fr/previous-versions/tn-archive/aa995740(v=exchg.65), microsoft, consulté le 27/08/23, date article : 25/07/2014
[4] https://learn.microsoft.com/fr-fr/exchange/policy-and-compliance/smime/smime?view=exchserver-2019, microsoft, consulté le 27/08/23, date article : 04/04/2023
