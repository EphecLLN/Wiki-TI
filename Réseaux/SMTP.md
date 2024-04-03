---
layout: default
title: SMTP
parent: Réseaux
---
# Protocole SMTP

## Qu'est ce que SMTP (Simple Mail Transfer Protocol) ? [2][3][4]

Le Simple Mail Transfer Protocol (SMTP) est un protocole utilisé pour transmettre des courriers électroniques sur un réseau. Il permet aux ordinateurs et aux serveurs d'échanger des données de manière normalisée, indépendamment de leur configuration matérielle ou logicielle. Le SMTP facilite la circulation des e-mails entre l'expéditeur et le destinataire, de manière similaire à l'adressage standardisé sur une enveloppe utilisé par le service postal. Cela rend possible la transmission de courriers électroniques à grande échelle.

Cependant, il est important de noter que le SMTP est un protocole de transmission des e-mails et non de réception. Tout comme le service postal dépose le courrier dans une boîte aux lettres, le serveur SMTP transmet l'e-mail au serveur du fournisseur de messagerie. Le destinataire doit ensuite utiliser d'autres protocoles (POP/IMAP) pour récupérer l'e-mail depuis le serveur afin de le lire.

## Comment fonctionne le SMTP ? [1][2][3][4][5]

Les protocoles de mise en réseau suivent tous une procédure établie pour l'échange de données. Le protocole SMTP, par exemple, définit la manière dont les données sont échangées entre un client de messagerie et un serveur de messagerie. Le client de messagerie est l'interface permettant à un utilisateur d'interagir avec l'application de messagerie utilisée pour lire et envoyer des e-mails, qu'il s'agisse d'une application de bureau ou web. Le serveur de messagerie est un ordinateur spécialisé dans l'envoi, la réception et le transfert des e-mails. Les utilisateurs n'interagissent pas directement avec ces serveurs.

Voici les étapes qui se déroulent entre le client de messagerie et le serveur de messagerie pour envoyer un e-mail :

1. **Ouverture de la connexion SMTP** : étant donné que le SMTP utilise le protocole TCP (Transmission Control Protocol) comme protocole de transport, la première étape consiste à établir une connexion TCP entre le client et le serveur. Ensuite, le client de messagerie lance le processus d'envoi de l'e-mail en exécutant une commande spéciale appelée "Hello" (HELO ou EHLO).

2. **Transfert des données de l'e-mail** : le client envoie une série de commandes au serveur, accompagnées du contenu de l'e-mail lui-même. Cela comprend l'en-tête de l'e-mail (contenant notamment le destinataire et l'objet), le corps de l'e-mail et les éventuelles pièces jointes.
  
3. **Exécution de l'Agent de Transfert de Courrier (Mail Transfer Agent)** : le serveur exécute un programme appelé Mail Transfer Agent (MTA), qui vérifie le domaine de l'adresse e-mail du destinataire. Si ce domaine est différent de celui de l'expéditeur, le MTA interroge le DNS (Domain Name System) pour obtenir l'adresse IP du destinataire. Cela peut être comparé à la recherche du code postal d'un destinataire par les services postaux.

4. **Fermeture de la connexion** : une fois que le client a terminé la transmission des données, il informe le serveur, qui clôt alors la connexion. À ce stade, le serveur ne reçoit plus de données supplémentaires concernant l'e-mail, sauf si le client établit une nouvelle connexion SMTP.

Le premier serveur de messagerie n'est généralement pas la destination finale de l'e-mail. Une fois que le client a envoyé l'e-mail au premier serveur, ce dernier répète la procédure de connexion SMTP avec un autre serveur de messagerie. Ce processus se répète jusqu'à ce que l'e-mail atteigne finalement la boîte de réception du destinataire, sur un serveur de messagerie contrôlé par le fournisseur de messagerie du destinataire.

Pour mieux comprendre on peut comparer ce processus à la façon dont un courrier estcheminé entre l'expéditeur et le destinataire. Le facteur ne transporte pas directement une lettre de l'expéditeur au destinataire, mais la remet d'abord à son bureau de poste. Ensuite, le bureau de poste envoie la lettre à un bureau situé dans une autre ville, qui la renvoie à un autre bureau, et ainsi de suite, jusqu'à ce que la lettre atteigne sa destination. De la même manière, les e-mails passent d'un serveur à l'autre via le protocole SMTP jusqu'à ce qu'ils parviennent à la boîte de réception du destinataire.

## Les commandes SMTP [2][3][4]

Les commandes SMTP sont des instructions prédéfinies sous forme de texte, utilisées pour indiquer à un client ou à un serveur ce qu'il doit faire et comment traiter les données accompagnant un e-mail. On peut les comparer à des boutons sur lesquels le client peut appuyer pour s'assurer que le serveur accepte les données correctement.

Voici les commandes principaux :

* **HELO/EHLO** : Ces commandes permettent d'établir la connexion SMTP entre le client et le serveur en disant "Bonjour" (HELO) ou en utilisant une version spécialisée du SMTP (EHLO).

* **MAIL FROM** : Cette commande informe le serveur de l'expéditeur de l'e-mail. Par exemple, si Alice souhaite envoyer un e-mail à son ami Bob, le client enverrait la commande "MAIL FROM:alice@exemple.com".

* **RCPT TO** : Cette commande spécifie la liste des destinataires de l'e-mail. Si l'e-mail a plusieurs destinataires, le client peut envoyer cette commande plusieurs fois. Dans l'exemple précédent, le client de courrier électronique d'Alice enverrait "RCPT TO:bob@exemple.com".

* **DATA** : Cette commande précède le contenu de l'e-mail.

* **RSET** : Cette commande réinitialise la connexion et supprime toutes les informations précédemment transférées, sans mettre fin à la connexion SMTP. Elle est utilisée lorsque le client a envoyé des informations incorrectes.

* **QUIT** : Cette commande met fin à la connexion SMTP.

Voici un exemple pour mieux comprendre les différentes commandes :

![image](https://d6x8u9i2.rocketcdn.me/blog/wp-content/uploads/2017/11/SMTP-sequence-diagram.png)

_source : https://www.afternerd.com/blog/smtp/_

## Qu'est-ce qu'un serveur SMTP ? [1][4][5]

Un serveur SMTP est un serveur de messagerie qui permet l'envoi et la réception d'e-mails en utilisant le protocole SMTP. Lorsqu'un client souhaite envoyer un e-mail, il se connecte directement au serveur SMTP fourni par le service de messagerie. Le serveur SMTP exécute différents programmes :

* **Mail Submission Agent (MSA)** : Il reçoit les e-mails envoyés par le client de courrier électronique.

* **Mail Transfer Agent (MTA)** : Il transfère les e-mails vers le serveur suivant dans la chaîne d'acheminement. Si nécessaire, il peut consulter le système de noms de domaine (DNS) pour trouver l'enregistrement MX (Mail eXchange) du domaine du destinataire.

* **Mail Delivery Agent (MDA)** : Il reçoit les e-mails envoyés par les MTA et les stocke dans la boîte de réception du destinataire.

## Quel port le protocole SMTP utilise-t-il ? [4][7]

Dans le domaine des réseaux, un port fait référence à un point virtuel de réception des données réseau. On peut le comparer au numéro d'appartement dans une adresse postale. Les ports permettent aux ordinateurs de trier et de transmettre les données réseau vers les applications appropriées. Les mesures de sécurité réseau, comme les pare-feu, peuvent bloquer les ports inutilisés afin d'empêcher l'envoi et la réception de données malveillantes.

Historiquement, le protocole SMTP utilisait uniquement le port 25. Ce port est toujours utilisé aujourd'hui par le protocole SMTP, mais il peut également utiliser les ports 465, 587 et 2525.

* Le port 25 est principalement utilisé pour les connexions entre serveurs SMTP. De nos jours, les pare-feu des réseaux des utilisateurs finaux bloquent souvent ce port, car les spammeurs tentent de l'utiliser de manière abusive pour envoyer d'importantes quantités de contenu indésirable.

* Le port 465 était autrefois assigné à l'utilisation du protocole SMTP avec chiffrement Secure Sockets Layer (SSL). Le SSL a depuis été remplacé par le protocole Transport Layer Security (TLS) et les systèmes de messagerie modernes n'utilisent plus ce port. Il est principalement présent dans les systèmes plus anciens (obsolètes).

* Le port 587 est désormais le port par défaut pour l'envoi d'e-mails. Les communications SMTP effectuées via ce port utilisent le chiffrement TLS.

* Le port 2525 n'est pas officiellement associé au protocole SMTP, mais certains services de messagerie proposent l'utilisation de la transmission SMTP via ce port en cas de blocage des ports mentionnés ci-dessus.

## Les inconvénients du protocole SMTP [3]

Le protocole SMTP présente deux inconvénients.

Le premier est qu'il ne fournit pas de confirmation d'expédition consultable lors de l'envoi d'un email. Bien que les spécifications du protocole prévoient une telle notification, son format n'est pas défini par défaut, ce qui signifie qu'en général, seul un message d'erreur en anglais incluant l'en-tête du message non délivré est renvoyé. Par conséquent, il est difficile de déterminer la cause de l'échec de la transmission, comme une adresse incorrecte ou une boîte de réception saturée du côté du destinataire.

Le deuxième inconvénient du SMTP est l'absence d'authentification des utilisateurs lors de l'établissement d'une connexion, ce qui rend l'identité de l'expéditeur d'un email peu fiable. Les relais SMTP ouverts sont souvent utilisés de manière abusive pour l'envoi massif de spams. Les expéditeurs utilisent des adresses d'expéditeurs fictives, ce qui rend leur traçabilité impossible (spoofing d'email). Aujourd'hui, de nombreuses techniques de sécurité sont mises en œuvre pour prévenir une utilisation abusive des serveurs SMTP. Par exemple, les emails suspects sont rejetés ou placés en quarantaine (dossier spam). Le protocole d'identification DomainKeys, le Sender Policy Framework (SPF) et le Greylisting sont utilisés à cette fin. De plus, il est devenu courant de recevoir des emails non seulement via le port traditionnel 25/TCP, mais aussi via d'autres ports.

## Qu'est-ce que l'Extended SMTP (ESMTP) ? [3][4]

L'ESMTP (Extended Simple Mail Transfer Protocol) est une version améliorée du protocole de transfert de courrier électronique, qui étend les fonctionnalités de la version initiale. Il permet notamment l'envoi de pièces jointes et l'utilisation du TLS, entre autres fonctionnalités. Contrairement au SMTP classique, la plupart des clients et services de messagerie utilisent l'ESMTP.

L'ESMTP introduit des commandes supplémentaires, telles que la commande "EHLO" qui permet l'envoi d'un message de salutation étendu (extended hello), afin d'activer l'utilisation de l'ESMTP dès le début de la connexion.

## Sécurisation du protocole SMTP [8]

Dans tous les protocoles de communication sur Internet, garantir l'intégrité, la confidentialité et l'authenticité des données est une priorité en matière de sécurité de communication. Il existe 2 techniques pour sécuriser le protocole SMTP.

1. **Le chiffrement des mails** : La technique de chiffrement des données est utilisée par les utilisateurs du protocole SMTP pour éviter d'envoyer des e-mails en clair sur le réseau. Elle permet de créer un canal de communication sécurisé entre le client de messagerie et les serveurs SMTP.

Parmi les méthodes de chiffrement les plus couramment utilisées, on trouve le Protocole TLS (Transport Layer Security) et le standard S/MIME (Secure/Multipurpose Internet Mail Extensions). Ces méthodes garantissent la confidentialité et l'intégrité des données échangées, empêchant ainsi leur lecture par des tiers. 

2. **L’authentification SMTP** : L'authentification SMTP, également connue sous le nom de SMTP AUTH, est une extension conçue pour protéger un serveur de messagerie contre une utilisation non autorisée. Afin d'envoyer un e-mail via le serveur SMTP et de procéder à son envoi, l'expéditeur doit s'authentifier en utilisant un compte valide. Cela réduit la possibilité d'envoi de spam à partir de clients non autorisés.

Cependant, il est important de noter que l'extension SMTP AUTH ne contrôle que l'envoi de l'e-mail et non l'ensemble de ses informations. Par conséquent, elle ne garantit pas l'identité de l'expéditeur affichée dans l'en-tête "From". Il est donc possible pour une personne malveillante de se faire passer pour quelqu'un d'autre, malgré cette couche de sécurité. 

## Comment tester un serveur SMTP ? [6][9]

Pour effectuer un test sur un serveur SMTP, la méthode la plus recommandée consiste à utiliser Telnet, un protocole largement utilisé dans les réseaux TCP/IP.

Voici un exemple qui illustre comment effectuer un test de connexion SMTP depuis un client interne vers un serveur, en utilisant une authentification standard sous Windows :

1. Ouvrez l'invite de commande en recherchant le terme "cmd" dans la barre de recherche.

2. Tapez la commande "telnet smtp.exemple.com 25" pour vous connecter au serveur SMTP via le port 25. Remplacez "smtp.exemple.com" par l'adresse de votre propre serveur SMTP.

3. Si le serveur est accessible, il renverra un rapport avec les codes d'état 220 et "smtp.example.com ESMTP Postfix", ou un message en texte clair équivalent. Cela indique qu'il n'y a pas d'erreur de connexion de la part du serveur SMTP.

Ensuite, vous pouvez vous authentifier et, si nécessaire, envoyer un e-mail de test pour mieux cerner la cause du problème. Si malgré une connexion fonctionnelle, l'e-mail n'atteint pas sa destination, le problème est probablement lié au fournisseur ou au destinataire.

Si vous ne recevez aucune réponse ou seulement un message d'erreur du serveur, il est donc possible que votre pare-feu ou votre programme antivirus bloque la transmission d'e-mails.

## Bibliographie

* [1] Numa, [SMTP : définition, serveurs, protocole… Le guide pour s’y retrouver !](https://www.brevo.com/fr/blog/smtp-definition-protocole-serveurs/), version du 23 août 2017, consultée le 25 mai 2023

* [2] Wikipedia, [Simple Mail Transfer Protocol](https://fr.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol), version du 26 juillet 2022, consultée le 25 mai 2023

* [3] Ionos, [Qu’est-ce que SMTP ? Définition et principes de bases](https://www.ionos.fr/digitalguide/email/aspects-techniques/smtp/), version du 25 juin 2019, consultée le 25 mai 2023

* [4] CloudFlare, [Qu'est-ce que le Simple Mail Transfer Protocol (SMTP) ?](https://www.cloudflare.com/fr-fr/learning/email-security/what-is-smtp/), consultée le 25 mai 2023

* [5] ActiveTrail, [SMTP: Définition Et À Quoi Ça Sert ?](https://www.activetrail.fr/blog_marketing/email_marketing_articles_fr/smtp-definition-et-a-quoi-ca-sert/), version du 9 août 2022, consultée le 25 mai 2023

* [6] AUDREY TIPS, [Compte SMTP (SMTP)](https://audreytips.com/glossaire-web/compte-smtp/), version du 16 juillet 2020, consultée le 25 mai 2023

* [7] Florian Burnel, [Messagerie : découverte des protocoles SMTP, POP, IMAP et MAPI](https://www.it-connect.fr/messagerie-decouverte-des-protocoles-smtp-pop-imap-et-mapi/), version du 7 janvier 2021, consultée le 25 mai 2023

* [8] Anne Quan, [Tout ce que vous devez savoir sur la sécurité SMTP](https://pacomail.io/blog/securite-smtp/), version du 5 september 2022, consultée le 25 mai 2023

* [9] Ionos, [Serveur SMTP : définition et fonctionnement](https://www.ionos.fr/digitalguide/email/aspects-techniques/serveur-smtp/), version du 26 juin 2019, consultée le 25 mai 2023