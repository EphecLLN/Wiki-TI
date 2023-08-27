---
layout: default
title: DMARC
parent: Réseaux
---
# DKIM [^1]

## qu'est ce que c'est?
DKIM ou "Domain Keys Identified Mail" est une solution au problème d'authentification de l'envoyeur d'un mail. celui-ci, vise à corriger deux problèmes inhérent a SMTP, la prévention de Spoofing (usurpation d'identitée) et la datection d'alteration durant le transit.

Pour faire une analogie, puis que SMTP à l'origine ce base sur le "Snail Mail" ou courier papier aucune mesure de sécuritée n'était mise en place car il est possible d'écrire n'importe quois sur une enveloppe et pour autant que l'adresse de destination est correcte. le mail arive a destination. Le spoofing s'aparente a écrire l'adresse de quelqun d'autre au dos de l'enveloppe. tandis-que la détection d'alteration s'apparente au sceau de cire sur le courier officiel. 

Il est cependent important de noter que DKIM ne permet que d'autentifier le domaine c'est a dire le @gmail.com par exemple.

## le fonctionnement
comme pour tout en cryptographie, on commance par générer un paire de clef (une privée et une publique). Ces clef ne sont générée qu'une fois pour tout le domaine.

### 1. signer les email sortant
la signature se fait en deux étapes

#### canonicalisation et hash
la canonicalisation une étape crusiale puis qu'elle permet de s'assurer que tout les mails sont sous le même format. une série de choses seront alor changer dans le mail et son header:
- les espace au début et a la fin du mail sont retiré
- les espaces en fin de lignes sont retiré
- les séparateur de lignes sont tous remplacé par un `CR+LF` ou carriage return et line feed

sans cela, il ne serait pas possible de s'assurer que le resseveur du mail obtiennent bien le même *hash*.

une fois que le mail et son header son uniformisé, il est hash avec *SHA-256* qui va produire une suite de *256 bit* qui représente le mail. Il s'agit de son emprunte digitale en quelques sortes. Ce hash sera ajouté au header avant l'envois du mail.

**note:** par principe on considère que les hash sont unique pour chaque texte en entrée, ce n'est techniquement pas le cas mais l'important est que si on prend un texte qui difère par ne fus qu'un caractère le hash sera complètement diférent. ici il servira donc a s'assurer que le mail n'a pas été modifié.


#### signature
la signature est générée avec un algorithme tel que *RSA* qui a partir de donée et d'une paire de clef premet d'encrypter les donée avec un clée et de les décrypter avec la seconde.
dans ce cas ci, le hash généré juste avant sera utilisé comme donée et encrypter avec la *clef privée*. il ne sera donc possible d'obtenir le hash original qu'avec la clef publique. Le résultat de cet encodage, est la sigature et elle sera ajoutée avec le hash vu plus haut au header du mail puis envoyé.

### 2. la clef publique
la comunication de la clef publique ce fait par le biais de text records dans le DNS du domaine d'origine. cela permet de s'assurer que la clef émanne bien du domaine. 

voici un RR typique pour DKIM:
```
v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC5VGAHh6R5pL8RJd3D5jZXM7sDn9z9V
yNRsMv0b/dMzPdtdpnp3PRUGb0NwWvCj+p0dHWfInEbyb3FgOJL8DS/6e8vG+icHMLf0O3
C4iDuhm3f8C2xjTVYiKZVHf6YI9YYvT24jO1PBHrS25V6u6p3slz5FYhgcTQIDAQAB;
```
- `v` décrit la version: ici DKIM 1
- `k` est le protocole utilisé pour la génération de signature: ici RSA
- `p` est la clef publique.

### 3. réception et vérification
une fois le mail reçu, deux vérification doivent avoir lieux mais avant il faut récupèrer la *clef publique* auprès du DNS. une foit que c'est fait:

#### le hash
pour s'assurer que le mail n'a pas subis de modifications durant le transit, le hash initial est a nouveau calculé en ométant le hash déja calculé et la signature puis qu'ils n'était pas présent lors du calcul initial.
Une fois le seconf hash obtenu il suffit de comparer les deux, si ils ne sont pas identique, c'est qu'une modification a été effectuée sur le mail.

#### la signature
il reste maintenant a valider la signature pour cela il faut repasser la signature dans l'algorithme *RSA* mais cette fois avec la clef publique ce qui doit nous donner le hash d'origine. Ce hash est comparé avec celui généré lors de l'étape précédante. Si il corresponde c'est que le mail provient bien du domaine précisé comme envoyeur du mail.

### 4. prise de décision
une fois les deux tests effectué le serveur doit décider si il accepte ou non le mail plusieurs cas sont possibles:

- **"DKIM Authentication Passed"** signifie que le mail a été validé et son origine confirmée. il sera surement pacé dans la boite de réception du destinataire directement.
- **"DKIM Authentication Failed"** signifie ue la signature est manquante ou n'a pas pu être validée. La le comportement dépend grandement de la configuration du serveur mail. par exemple il peut être marqué comme suspicieux ou encore placé dans les couriers indésirables.
- **le cas DMARC** si le domaine d'origine stipule des règles *DMARC* plus strictes elle prendront la prioritée sur le traitement du mail. Il peut être mis en quarantaine ou tout simplement rejeter.
- **autre** il existe un grand nombres de facteur que le serveur peut prendre en compte dans ca désision. tel que la notoriétée du domaine d'origine, son adresse ip, SPF, ou tout simplement des préférances utilisateurs.

## Bibliographie

[^1]:   D. Crocker, T. Hansen, M. Kucherawy [RFC6376 - DomainKeys Identified Mail (DKIM) Signatures](https://www.rfc-editor.org/rfc/rfc6376.html) September 2011
    
       **Résumé** : RFc décrivant les principes de DKIM
       **Avis sur la ressource** : article d'authoritée