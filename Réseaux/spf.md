[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)

---

layout: default
title: SPF
parent: Réseaux

---

# SPF

## Qu'est ce que c'est?

SPF, acronyme de Sender Policy Framework, permet d'empêcher les envois de message par des spammeurs au nom de votre domaine au moyen d'un système de validation. Spf permet a une organisation de publier des servers de messagerie autorisés. Ce framework fonctionne de pair avec DMARK. Il donne à ceux qui recoivent l'information les différentes informations par rapport à la fiabilité de l'origine du mail. [1]

Il s'agit donc d'un moyen d'autentification par nom de domaine pour éviter l'usurpation d'identité. Pour cela, il a besoin du DNS. Ainsi, tout expéditeurs peux spécifier quels serveurs peuvent envoyer des e-mails au nom de leur domaine.

## Histoire

Les premières mentions de SPF remontent au environ de 2000. La spécification de ce dernier s'est faite dans les années suivantes au moyen de plusieurs versions. Le nom original de SPF était tout d'abord "Sender Permitted Form". Ce dernier a donc évolué durant les premières versions. [1]

En 2006, une tentative de combinaison de SPF avec la proposition CallerId de Microsoft a été réalisée par un groupe de travail de l'[IETF](https://www.ietf.org/). Cette expérience aura porté ses fruits en 2014 et est maintenant connue sous le nom de RFC 7208.[1]

Actuellement, les techniques d'autentification privilègient DKIM et DMARC par rapport à SPF. Cependant, il détermine toujours si un mail est conforme au DMARC.[1]

## Dans la pratique

Pour configurer SPF, il faut au préalable ajouter un SPF record dans la zone DNA du domaine. Celui-ci permet la spécification des différentes adresses ips qui sont authorisées à envoyer des mails au nom du domaine.[1]

SPF vérifie qu'un expéditeur peux envoyé des messages avec ce nom de domaine. Si pas (la vérification a échouée), le serveur récepteur détermine ce qu'il faut faire du message en fonction de la stratégie configurée.[4]

![SPF](./\Schema_SPF.jpg)

* Nous pouvons donc comprendre grâce au schéma ci-dessus, lorsque nous recevons un mail, il est d'abord soumis a la vérification SPF avnt d'arriver sur notre boite mail. ce qui empêche l'usurpation d'identité.

## SPF record

le SPF record est composé de plusieurs parties. Il commence par un numéro de version, suivi par les différents mécanismes qui permettent de de définir les expéditeurs valides.[3]
type de SPF record : `"v=spf1 ip4:207.171.160.0/19 -all"`

* numéro de version : `v=spf1`

### mécanismes

Dans un enregistrement DNS SPF, il peux y avoir différents mécanismes utilisés qui permettent au serveur récepteur de savoir quel mécanisme d'authentification il devra utilisé.[5]

* a : défini le record du DNS A du domaine comme source d'envoi valide.
* mx : défini le record MX du domaine comme source d'envoi valide.
* ptr : défini le nom d'hôte inverse de l'adresse ip d'envoi comme source d'envoi valide.
* ip4 : défini l'adresse/pool d'adresse ipv4 comme source d'envoi valide.
* ip6 :  défini l'adresse/pool d'adresse ipv6 comme source d'envoi valide.
* include : inclu le SPF record pour le nom de domaine.

il est également possible de rajouter des qualificateurs qui permettent de spécifier la portée de l'enregistrement. On les utilise principalemrnt pour spécifié si un addresse ip peux ou pas envoyé des mail avec notre nom de domaine.

## exemples

`v=spf1 a:mail.solarmora.com ip4:192.72.10.10 include:_spf.google.com ~all` pour cet enregistrement, les expéditeurs suivant sont authorisé a envoyé des mails :

* le serveur mail.solarmora.com
* le serveur disposant de l'adresse ip 192.72.10.10
* Google Workspace
  
une fois que l'enregistrement SPF est fait, il faut l'ajouter a notre domaine

`TXT @ v=spf1 include:_spf.google.com ~all 3600` dans ce cas ci, nous autorisons l'envoi des ail a partir de google Workspace
TXT fait référence au  type d'enregistrement
@ fait référence au domaine. si il y a un sous-domaine, le mettre ici au lieu du `@`.
`v=spf1 include:_spf.google.com ~all` ceci fait référence a la valeur de l'enregistrement
3600 fait référence au TTL.

## Limitation

Le spf record a cependant quelques problèmes.

* la validation se fait uniquement sur "enveloppe de" et donc l'"en-tête de" n'est pas validée.[1] alors que dans la plupart des clients, c'est "en-tête de" qui est montré comme expéditeur.
* lors du transfert d'un mail, la personne qui transfert deviens de nouvel expéditeur et donc, les vérification SPF échouent.[1]
* Le maintien est plus difficile dû au manque de rapports.[1]

## sources

* [1] Qu'est-ce que le SPF ? - DMARC Analyzer. (s. d.). DMARC Analyzer. <https://www.dmarcanalyzer.com/fr/spf-4/> consulté le 27-05-2022
* [3] Qu'est-ce qu'un SPF record? - DMARC Analyzer. (n.d.). DMARC Analyzer. <https://www.dmarcanalyzer.com/fr/spf-4/spf-record/> consulté le 05-06-2022
* [RFC SPF] RFC 7208
* [RFC Dmark] RFC 7489
* [4] <https://docs.microsoft.com/fr-fr/microsoft-365/security/office-365-security/how-office-365-uses-spf-to-prevent-spoofing?view=o365-worldwide> consulté le 05-06-2022
* [5] La syntaxe de l'enregistrement SPF expliquée - Un guide simple de la syntaxe SPF. (n.d.). PowerDMARC. <https://powerdmarc.com/fr/spf-record-syntax/> consulté le 25-08-2022
