---
layout: default
title: SpamAssassin
parent: Réseau
---

# SpamAssassin

![spam-assasin-logo-transparent](https://github.com/HaAymar/Wiki-TI/assets/71372488/1942bfb7-2f7b-4608-86ca-b60ff8112e35)

## Définission SpamAssassin
SpamAssassin est un logiciel de filtrage du courrier indésirable (spams) qui est trés utilisé, il analyse les en-têtes et le contenu des e-mails pour détecter les signes de spam.
- Il utilise des listes noires pour identifier les expéditeurs et les serveurs de messagerie non fiables associés au spam
- SpamAssassin peut également analyser les pièces jointes et les URL dans les e-mails pour détecter les contenus suspects
- Il attribue des scores aux messages en fonction des règles et des analyses effectuées.

Il peut être intégré avec des serveurs de messagerie utilisant les protocoles POP, IMAP et SMTP.
### Difference entre POP/IMAP et SMTP

<b>POP/IMAP</b> : sont les protocoles de messagerie utilisés pour récupérer les e-mails à partir d'un serveur de messagerie.

<b>SMTP</b> :  est un protocole standard utilisé pour l'envoi d'e-mails à travers les réseaux. Il est principalement utilisé pour l'envoi de mails à partir d'un client de messagerie vers un serveur de messagerie sortant.

Afin de restreindre la propagation de spam, des filtres sont employés à divers niveaux comme : MUA, MTA, MDA ...
## Pourquoi utilisé le SpamAssassin

Les principales raisons qui poussent à l'utilisation de SpamAssassin sont les suivantes:

- <b>Protection contre les menaces</b> : SpamAssassin est capable de détecter et de marquer les e-mails contenant des liens malveillants, des pièces jointes dangereuses ou d'autres éléments associés à des attaques de phishing, de malware ou d'autres menaces en ligne. Cela contribue à renforcer la sécurité du système de messagerie.

- <b>Réduction du temps de traitement</b> : En filtrant automatiquement les e-mails indésirables, SpamAssassin permet de réduire la quantité de courrier indésirable qui atteint les boîtes de réception des utilisateurs. Cela permet aux utilisateurs de passer moins de temps à trier manuellement les e-mails non sollicités.

- <b>Personnalisation des règles</b> : SpamAssassin permet aux utilisateurs de personnaliser les règles de filtrage en fonction de leurs besoins spécifiques. Ils peuvent ajouter, supprimer ou ajuster les règles pour mieux s'adapter à leurs préférences et à leur environnement de messagerie.

## Installation et configuration de SpamAssassin
Avant de pouvoir configurer SpamAssassin il faut d'abord installer ça sur une machine.
Voici les étapes de configuration sous linux:

Installation spamAssassin

```
sudo apt-get install spamassassin
```

Après l'installation de toutes les dépendances, il faudra créer un utilisateur système qui pourra communiquer avec le démon `spamd`

```
adduser --disabled-login --home /var/spamd spamd
```

Configuration pour le fichier ```/etc/default/spamassassin``` on place le CRON à 1:

```
CRON=1
```
Aprés indiquer à Postfix  qu'on utilise SpamAssassin dans le fichier `/etc/postfix/master.cf`on ajoutant:
```
smtp      inet  n       -       -       -       -       smtpd
-o content_filter=spamassassin
```
Et les deux lignes à la fin de ce même fichier:

```
spamassassin unix -     n       n       -       -       pipe
 user=spamd argv=/usr/bin/spamc -f -e /usr/sbin/sendmail -oi -f ${sender} ${recipient}
```
En suite demarrer le SpamAssassin avec la commande:
```
systemctl start spamassassin
postfix reload
```
[Sources](https://www.hostnextra.com/kb/install-spamassassin-with-postfix-on-ubuntu/)

- [Utilisation SpamAssassin windows](https://cwiki.apache.org/confluence/display/spamassassin/UsingOnWindows)
- [Utilisation SpamAssassin avec centOs](https://archives.microlinux.fr/spamassassin-centos-7/)

## Comment integrer SpamAssassin dans les protocoles POP/IMAP et SMTP

Pour intégrer SpamAssassin dans les protocoles POP/IMAP, il faut suivre ces étapes suivantes :
- Installez SpamAssassin sur la machine (voir l'étape précedente)
- Configurez votre serveur de messagerie pour acheminer les e-mails via SpamAssassin.
- Activez le support SpamAssassin dans la configuration du serveur de messagerie.
- Configurez le serveur de messagerie pour appliquer les filtres SpamAssassin aux e-mails entrants.
- Définissez des actions en fonction des résultats du filtrage (marquage, déplacement des e-mails indésirables).
Il est possible de consulter la documaentation en suivant ce lien en cliquant [ici](https://doc.ubuntu-fr.org/serveur_mail_avec_postfix_et_courier-imap)

Pour intégrer SpamAssassin dans SMTP, vous pouvez suivre les étapes suivantes : 
Pour faire la configuration, il faut ouvrir le fichier sur le serveur SMTP qui se trouve dans `/etc/postfix/main.cf`
On ajoute une ligne :
```
content_filter = smtp:[127.0.0.1]:10025
```
On ajoute une nouvelle section pour définir le filtrage des e-mails avec SpamAssassin
```
smtpd_proxy_filter = inet:127.0.0.1:10026
```
On enregistre les modifications et redémarrez le serveur SMTP pour appliquer la configuration
```
systemctl restart postfix
```
Enfin on démarre le démon SpamAssassin sur le port 10026
```
sudo spamd -d -c -m5 -H -s spamd -u spamd -x -r /var/run/spamd.pid
```
## Conclusion
SpamAssassin est une solution efficace et largement utilisée pour filtrer le courrier indésirable dans les services de messagerie. Avec son intégration transparente avec les protocoles POP, IMAP et SMTP, SpamAssassin offre une protection complète contre les spams, les e-mails malveillants et les menaces en ligne

## Bibliographie
- [1] [C'est quoi spamAssassin](https://en.wikipedia.org/w/index.php?title=Apache_SpamAssassin&action=history) consulté le 25/05/2023
- [2] [Installation et configuration de SpamAssassin](https://www.hostnextra.com/kb/install-spamassassin-with-postfix-on-ubuntu/)consulté le 25/05/2023
- [3] [Comment integrer SpamAssassin dans les protocoles POP/IMAP et SMTP](https://siguillaume.developpez.com/tutoriels/linux/mise-place-systeme-messagerie-electronique-sous-linux/?page=page-1)consulté le 25/05/2023
- [4] [Configuration smtp avec SpamAssassin](https://www.malekal.com/installer-configurer-spamassassin-debian/)consulté le 25/05/2023
- [5] [Protocole SMTP ET POP/IMAP](https://www.cleanfox.io/blog/divers/les-protocoles-de-messagerie/#:~:text=Les%20protocoles%20sortants%20(SMTP)%20servent,et%20de%20distribution%20des%20messages) consulté le 26/05/2023
- [6] [Pourquoi utilisé SpamAssassin](https://www.hostpapa.com/knowledgebase/fr/knowledge-base/comment-utiliser-spamassassin/) consulté le 26/05/2023
