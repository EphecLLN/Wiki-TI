---
layout: default
title: Configuration d'un serveur Postfix
parent: Administration Systèmes
---

# Configuration d'un serveur postfix

## Introduction

[Postfix][postfix] est [Mail Transfer Agent (MTA)][wiki-mta] Open Source
disposant d'une grande quantité de resource. Né en 1998, il a pu prouver qu'il
est un serveur SMTP performant, sécurisé et robuste.

Postfix supporte de nombreuse features, telles que LDAP, des services antispam
et/ou antivirus (comme [Rspamd][rspamd]), ou un [Mail Delivery Agent (MDA)]
[wiki-mda] tel que [Dovecot][dovecot].

Postfix est constitué d'une série de services tournant en arrière plan et
formant une pipeline. Chaque service a la possibilité de s'arrêter en cas
d'erreur ou d'opération incomplète ou invalide. Un mail traversant la pipeline
sera alors laissé au service précédent dans la pipeline, qui pourra redémarrer
le(s) service(s) adéquat plus tard pour terminer le traitement du mail.

![Diagramme d'architecture de postfix](https://upload.wikimedia.org/wikipedia/commons/5/53/Architecture_of_the_software_Postfix_%28Mail_Transfer_Agent%29.png)
*image sous license [CC-BY-SA 3.0][cc-by-sa-30]*
*source: [Wikipedia Commons](https://en.wikipedia.org/wiki/File:Architecture_of_the_software_Postfix_(Mail_Transfer_Agent).png)*

## Fichiers de configuration

Le fichier de configuration principal de postfix est [`main.cf`][main-cf]. Ce
fichier suit une syntaxe semblable aux schéma des fichiers `.ini`. Il configure
les paramètres globaux des opérations du serveur.

Un autre fichier, [`master.cf`][master-cf], déclare et configure les services
de postfix. D'autres fichiers de configuration existent également pour certains
services spécifiques, notamment optionels tels qu'un MDA ou un antispam tierce.
Nous allons ici nous concentrer sur `main.cf`.

> Le dossier ou postfix trouve ses fichiers de configuration est `/etc/postfix`
> par défaut, mais peut être spécifié par la ligne de commande avec l'option
> `-c`.

## Configurer postfix

Pour la suite, nous allons considérer que la machine hébergant postfix a le
hostname `mail` et se trouve dans la zone `example.org`. Nous allons aussi
supposer qu'un record DNS `MX` pointe vers `mail.example.org` dans la zone
`example.org`.

> Tip: La commande `postconf` permet de query la configuration (globalle, ou
> celle dans le dossier spécifié par l'option `-c`).
> Il est particulièrement utile d'utiliser l'option `-d` pour voir les
> paramètres par défaut, et `-n` pour voir les paramètres explicitement
> spécifiés par la configuration.
> Combiné avec `grep`, c'est un outil puissant pour examiner l'état de votre
> configuration. Par exemple pour examiner quels paramètres utilise la valeur
> `$myhostname`: `postdonf | grep '$myhostname'`.

### Le domaine

Le paramètre [`myhostname`](https://www.postfix.org/postconf.5.html#myhostname)
définit le nom de la machine pour postfix. Il est par défaut par une variété
de paramètres. Si ommit, il prend la valeur du hostname de la machine.

> Par convention, le hostname d'une machine est lisible dans `/etc/hostname`.

Un autre paramètre souvent utilisé par défaut est `mydomain`, correspondant à
la zone parente de la machine. Postfix ne peut pas la connaitre par lui-même
et dépend de la résolution de nom de l'OS, qui pourra transmetre une variété de
réponses. Ces mécanismes ne sont pas toujours fiable (ou sont fragile), il vaut
donc mieux explicitement spécifier la bonne valeur.

> L'OS ne connait souvent pas sa zone parente, si il n'a pas été configuré
> explicitement. Il va donc souvent renvoyer une réponse telles que
> "localdomain", ou faire une requête DNS inverse. Il est courant de se
> retrouver avec des noms de domaines abracadabrants si tout ça n'est pas
> correctement mis en place au préalable, et des changements dans ces
> infrastructures se refléront dans postfix. Il vaut donc mieux spécifier
> `mydomain` directement.

Nous avons donc déjà nos deux premiers paramètres:

```
myhostname = mail
mydomain = example.org
```

#### Emails sortants

le paramètre [`myorigin`](https://www.postfix.org/postconf.5.html#myorigin)
définit le domaine apparaissant dans les mails postés par postfix. Si ommit,
il prend la valeur de `$myhostname` (résultant en des adresses `xxx@mail`).
Nous allons donc lui donner la valeur `$mydomain` (pour des adresses
`xxx@example.org`):

```
myorigin = $mydomain
```

#### Emails entrants

Le paramètre `mydestination` déclare pour quel domaine postfix doit délivrer
des mails localement, au lieu de les transférer à une autre machine. Il est
très important de donner **tout** les noms possibles de la machine pour éviter
qu'un email soit coincé dans une boucle. Par défaut, il n'inclue que
`$myhostname`, `localhost` et `localhost.$myhostname`. Nous dev_ons ajouter
notre domaine:

```
mydestination = $myhostname localhost.$mydomain localhost $mydomain
```

### Relais des mails clients

Postfix relais les mails de n'importe quel clients de n'importe quel
réseau autorisé. Pour authoriser des mails de clients depuis des réseaux
non-autorisés, il faut activer [TLS pour postfix][postfix-tls].

#### Acceptation des mails

Deux paramètres régissent les réseaux autorisé: [`mynetworks`](https://www.postfix.org/postconf.5.html#mynetworks)
et [`mynetworks_style`](https://www.postfix.org/postconf.5.html#mynetworks_style).
`mynetworks_style` définit comment interpréter le paramètre `mynetworks`. Il
est recommendé de le mettre à `host` pour trust uniquement les machines dont
les adresses IP sont données. Si l'on a le controle d'un intranet et que l'on
souhaite autorisé n'importe quel client y étant connecté à envoyer des mails de
postfix, alors nous pouvons le mettre à `subnet` et donner le préfix du réseau
dans `mynetworks` (`192.168.0.0/28` dans notre exemple):

```
mynetworks_style = subnet
mynetworks = 127.0.0.0/8 192.168.0.0/28
```

#### Destination des mails

Par défaut, postfix refuse d'envoyer des mails qui ne sont pas destinés au
domaine `$mydestination`. C'est le paramètre `relay_domains` qui définit quels
domaines peuvent se faire transférer nos mail. Si on veut relayer les mails
destinés à d'autres domaines, vous pouvez les y ajouter:

```
relay_domains = $mydestination toto.be titi.dev
```

La valeur par défaut est cependant suffisante pour un serveur mail local
hébergant les mails d'un seul domaine.

#### Hôte relais

Postfix envoie directement les mails sur internet en espérant qu'ils
parviennent à destination, par défaut. Si on souhaite faire passer nos mails
par un "hub", on peut spécifier sont nom dans le paramètre `relayhost` (vide
par défaut):

```
relayhost = [mailhub.$mydomain] smtp-relay.google.com
```

> Tip: les crochets autour d'un domaine empêchent postfix de faire une requête
> pour le record DNS `MX` du domaine, et envoie directement à l'adresse du
> serveur. Si ils ne sont pas ajouté, postfix va d'abord trouver un serveur
> mail du domaine et lui envoyer le mail.

## Conclusion

Par défaut postfix associe une adresse mail à chaque utilisateurs dans le
fichier de password UNIX (`/etc/passwd`). Si tout s'est bien passé, il ne
vous reste qu'à créer des utilisateurs !

## Bibliographie

1. [Postfix.org][postfix], consulté le 2 Juin 2023
  - Résumé: Site et documentation de postfix.
   
[wiki-mta]: https://en.wikipedia.org/wiki/Message_transfer_agent
[miki-mda]: https://en.wikipedia.org/wiki/Mail_delivery_agent
[miki-mua]: https://en.wikipedia.org/wiki/Email_client
[postfix]: http://www.postfix.org/
[dovecot]: https://dovecot.org/
[rspamd]: https://www.rspamd.com/
[main-cf]: https://www.postfix.org/postconf.5.html
[master-cf]: https://www.postfix.org/master.5.html
[cc-by-sa-30]: https://creativecommons.org/licenses/by-sa/3.0/deed.en
[postfix-tls]: https://www.postfix.org/TLS_README.html
