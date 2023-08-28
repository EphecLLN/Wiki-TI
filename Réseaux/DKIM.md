---
layout: default
title: DMARC
parent: Réseaux
---
# DKIM [^1]

## qu'est ce que c'est?
DKIM ou "DomainKeys Identified Mail" est une solution au problème d'authentification de l'expéditeur d'un e-mail. Celui-ci vise à corriger deux problèmes inhérents à SMTP : la prévention du spoofing (usurpation d'identité) et la détection d'altération pendant le transit.

Pour faire une analogie, puisque SMTP à l'origine se base sur le "Snail Mail" ou courrier papier, aucune mesure de sécurité n'était mise en place, car il est possible d'écrire n'importe quoi sur une enveloppe et pour autant que l'adresse de destination soit correcte, l'enveloppe arrive à destination. Le spoofing s'apparente à écrire l'adresse de quelqu'un d'autre au dos de l'enveloppe, tandis que la détection d'altération s'apparente à un sceau de cire.

Il est cependant important de noter que DKIM ne permet que d'authentifier le domaine, c'est-à-dire le @gmail.com par exemple.

## anatomie d'un mail

Voici un exemple de mail et de ces métadonée typique.

le header:
```
From: John Doe <johndoe@example.com>
To: Jane Smith <janesmith@example.net>
Subject: Important Meeting Tomorrow
Date: Thu, 26 Aug 2023 15:30:00 +0000
Message-ID: <123456789@example.com>
Content-Type: text/plain; charset="UTF-8"
DKIM-Signature: v=1; a=rsa-sha256; d=example.com; s=selector1; bh=bf92b7a04358ea8b75207997f885f4fd8bfd3ccbdd75df31a2778c54e686f714; b=FfyYotkjLkod9F7JO3Bmmg==
```

Les données (séparées du header par une ligne vide):
```

Hi Jane,

I hope this email finds you well. I wanted to remind you about the important meeting scheduled for tomorrow at 10:00 AM. Please make sure to bring the updated presentation slides.

Best regards,
John
```

## le fonctionnement
Comme pour tout en cryptographie, on commence par générer une paire de clés (une privée et une publique). Ces clés ne sont générées qu'une fois pour tout le domaine.

### 1. signer les email sortant
La signature se fait en deux étapes:

#### canonicalisation et hash
La canonicalisation est une étape cruciale qui permet de s'assurer que tous les mails sont sous le même format. Cela est dû au fait que différents éditeurs de textes utilisent différents formats d'encodage. Une série de choses seront changées dans le mail et son header:
- Les espaces au début et à la fin du mail sont retirés.
- Les espaces en fin de lignes sont retirés.
- Les séparateurs de lignes sont tous remplacés par un `CR+LF` ou carriage return et line feed (`\r\n`).

Sans cela, il ne serait pas possible de s'assurer que le destinataire du mail obtienne bien le même *hash*.

Une fois que le mail et son header sont uniformisés, ils sont hashé séparément avec *SHA-256*, ce qui va produire deux suites de *256 bits* qui représente le mail. Il s'agit de son empreinte digitale en quelque sorte. Le hash du "body" ou corps sera ajouté au header avant l'envoi du mail.

**Note** : Par principe, on considère que les hachages sont uniques pour chaque texte en entrée. Ce n'est techniquement pas le cas, mais l'important est que si on prend un texte qui diffère ne fût-ce que par un caractère, le hachage sera complètement différent. Ici, il servira donc à s'assurer que le mail n'a pas été modifié.


#### signature
La signature est générée avec un algorithme tel que *RSA*, qui à partir de données et d'une paire de clés permet d'encrypter les données avec l'une et de les décrypter avec l"autre.
Dans ce cas-ci, les hash généré juste avant ainsi que diverses options de dkim seront utilisés comme données et encrypté avec la *clé privée*. Il ne sera donc possible d'obtenir les donée d'origine qu'avec la clé publique. La dernière étape consiste à encoder le résultat de *RSA* en *base-64* ce qui nous donne la signature.

La signature et le hash sont ajouté au header et le mail est envoyé.

#### le header
Comme dans l'exemple vus plus haut voici à quoi resemble le header pour DKIM:
`DKIM-Signature: v=1; a=rsa-sha256; d=example.com; s=selector1; bh=bf92b7a04358ea8b75207997f885f4fd8bfd3ccbdd75df31a2778c54e686f714; b=FfyYotkjLkod9F7JO3Bmmg==`

- `v` Indique la version, elle doit toujours être à 1 (il n'y a pas d'autres versions).
- `a` Indique l'algorithme utilisé pour la génération de la signature (les plus petits CPU peuvent utiliser *rsa-sha1*, mais c'est un risque de sécurité).
- `d` Le domaine auprès duquel la *clé publique* doit être récupérée.
- `s` Permet la sélection du bon enregistrement TXT sur le DNS.
- `bh` Le hachage du corps du mail.
- `b` La signature.

### 2. la clef publique
La communication de la clé publique se fait par le biais de `TXT` records dans le DNS du domaine d'origine. Cela permet de s'assurer que la clé émane bien du domaine.

Voici un `RR` typique pour DKIM:
```
v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC5VGAHh6R5pL8RJd3D5jZXM7sDn9z9V
yNRsMv0b/dMzPdtdpnp3PRUGb0NwWvCj+p0dHWfInEbyb3FgOJL8DS/6e8vG+icHMLf0O3
C4iDuhm3f8C2xjTVYiKZVHf6YI9YYvT24jO1PBHrS25V6u6p3slz5FYhgcTQIDAQAB;
```
- `v` décrit la version: ici DKIM 1
- `k` est le protocole utilisé pour la génération de signature : ici RSA.
- `p` est la clef publique.

### 3. réception et vérification
Une fois le mail reçu, deux vérifications doivent avoir lieu, mais avant il faut récupérer la *clé publique* auprès du DNS.

#### le hash
Pour s'assurer que le mail n'a pas subi de modifications durant le transit, les hash initiaux sont à nouveau calculés en omettant DKIM dans le header puisqu'il n'était pas présent lors du calcul initial.
Une fois les hashs obtenu, on compare les deux hash de body. Si ils ne sont pas identiques, c'est qu'une modification a été effectuée sur le mail.

#### la signature
Il reste maintenant à valider la signature. Pour cela, il faut repasser la signature dans l'algorithme *RSA*, mais cette fois avec la clé publique, ce qui doit nous donner les hashs d'origine ainsi que les options DKIM. Ces donée sont comparée avec celle reçue dans le mail et les hash généré juste avant. S'ils correspondent, c'est que le mail provient bien du domaine de lenvoyeur.

### 4. prise de décision
Une fois les deux tests effectués, le serveur doit décider s'il accepte ou non le mail. Plusieurs cas sont possibles :

- **"DKIM Authentication Passed"** Signifie que le mail a été validé et son origine confirmée. Il sera surement placé dans la boîte de réception du destinataire directement.
- **"DKIM Authentication Failed"** Signifie que la signature est manquante ou n'a pas pu être validée. Le comportement dépend grandement de la configuration du serveur mail. Par exemple, il peut être marqué comme suspicieux ou encore placé dans les courriers indésirables.
- **le cas DMARC** Si le domaine d'origine stipule des règles DMARC plus strictes, elles prendront la priorité sur le traitement du mail. Il peut être mis en quarantaine ou tout simplement rejeté.
- **autre** Il existe un grand nombre de facteurs que le serveur peut prendre en compte dans sa décision, tels que la notoriété du domaine d'origine, son adresse IP, SPF, ou tout simplement les préférences des utilisateurs.

## Bibliographie

[^1]:   D. Crocker, T. Hansen, M. Kucherawy [RFC6376 - DomainKeys Identified Mail (DKIM) Signatures](https://www.rfc-editor.org/rfc/rfc6376.html) September 2011
    
       **Résumé** : RFc décrivant les principes de DKIM
       **Avis sur la ressource** : article d'authoritée