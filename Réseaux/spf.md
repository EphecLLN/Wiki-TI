[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)

---

layout: default
title: SPF
parent: Réseaux
---

# SPF

## Qu'est ce que c'est?

SPF, acronyme de Sender Policy Framework, permet d'empêcher les envois de message par des spammeur au nom de votre domaine au moyn d'un système de validation. Spf permet a une organisation de publier des servurs de messagerie autorisée. ce framework fonctionne de paire avec DMARK. il donne a ceux qui recoivent l'information les différentes information par rapport a la fiabilité de l'origine du mail. [1]

Il s'agit donc d'un moyen d'autentification par email. Pour cela, il a beoin du DNS. Ainsi tout expéditeurs peux spécifié quel serveurs peuvent envoyé des e-mail au nom de votre domaine.

## Histoire

Les premières mention de SPF remontent au environ de 2000. la spécification de ce dernier se sont fait dans les années suivantes au moyen de plusieurs version. le nom original de SPF était tout d'abord "Sender Permitted Form". ce dernier a donc évolué durant les premières versions. [1]

En 2006, une tentative de combinaison de SPF avec la proposition CallerId de Microsoft a été réalisée par un groupe de travail de l'IETF. cette expérience aura perté ses fruits en 2014 et est maintenant connue sous le nom de RFC 7208.[1]

Actuellement, les techniques d'autentification privilègient DKIM et DMARC par rapport a SPF. Cependatn, il determine toujours si un mail est conforme au DMARC.[1]

## Dans la pratique

POur configurer SPF, il faut au préalable ajouter un SPF record dans la zone DNA du domaine. Celui-ci permet la spécification des différentes addresses ips qui sont authorisés a envoyé des mails au nm du domain.[1]

Lors de la reception, la machine destinataire utilisera l'adresse "enveloppe de" du mail afin de vérifier l'authorisation de l'adresse IP source. Lors de l'affichag complet du mail, cela sera déjà fait. si l'addresse IP source ne figure pas dans le SPF record alors, l'e-mail sera désigné comme suspect et peux eventuellement être rejeté.[1]



## Limitation

* la validation se fait uniquement sur "enveloppe de" et donc l'"en-tête de" n'est pas validée.[1]
* lors du transfert d'un mail, la personne qui transfert deviens de nouvel expéditeur et don, les vérification SPF plante.[1]
* Le maintien est plus difficiel du au manque de rapports.[1]
* SPF vérifie uniquement le nom de domaine, mais pas l'utilisateur mail(avant le`@`')[2]

## sources

* [1] Qu'est-ce que le SPF ? - DMARC Analyzer. (s. d.). DMARC Analyzer. https://www.dmarcanalyzer.com/fr/spf-4/ consulter le 27-05-2022
* [2] https://fr.wikipedia.org/w/index.php?title=Sender_Policy_Framework&oldid=193547601 consulter le  27-05-2022
