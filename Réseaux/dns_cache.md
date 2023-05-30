---
layout: default
title: Cache DNS
parent: Réseaux
---
# Cache DNS

## Pourquois?

la nécécitée de cahe le DNS devient vit apparent quand on réfléchit en termes du nombre de querry qu'un resolveur doit faire pour obtenir la réponse voulue par l'utilisateur. Au minimum 2-3 et parfois bien plus de requète sont nécésaire entre les différents serveurs d'authoritée. ce qui devient fortement redondant si l'on prend l'exemple d'un utilisateur voyageant antre deux pages d'un même site ou deux service d'un même domaine. C'est donc pour aléger l'impact sur le réseau et accélérer les temps de réponces que les RR sont mis en cache a plusieurs étapes du processus.

## Fonctionnement [^1][^2]

### Structure du cache

le cache est généralement stoqué dans une structure de donée, plus particulièrement, un tableau de hash.
dans un tableau de hash les noms de domaines ne sont pas écrit tels quels mais sont converti grace a un algorithme de hachage. L'avantage de procéder comme ca est que la lecture des resources se fait en temps constant au lieu de linéaire.
les ressources stoquée sont les suivantes:
- le hash du nom de domaine, pour la récuperation en temps constant
- le RR ressu lors de la résolution
- l' IP du serveur d'authoritée, nécésaire pour la mise a jour des donée
- le TTL, nombre entier de seconde que la ressource est authorisée a vivre
- le timecode, souvent également en seconde, c'est la date et heure a laquelle la ressource a été rajoutée.

### Gestion du TTL

le TTL n'étant qu'un entier représentant le nombre de secondes durant laquelle la resource est considérée valide, il ne se suffit pas a lui même. lors du cache d'une nouvelle entrée, un timecode est stoqué avec le ttl et le reste des donée.

puis, lors de la résolution, le TTL et le timecode sont comparé. on soustrait le timecode au temps actuel, si le résultat est plus grand que le TTL alor la ressource n'est plus valide.

si un TTL expire, deux comportements sont possibles:
- le résolveur suprimme l'enter du cache. la requète suivante pour se domaine causera donc une résolution standard et une remise en cache des nouvelle donée.

- le résolveur tente de metre a jour ces donée. il envois une requète au serveur d'autoritée pour voir si une nouvelle version des donée est disponible. il met a jour l'entrée du cache et reset le TTL.
cette aproche permet d'éviter de complètement refaire la résolution DNS si les information n'ont pas changé.

## Limitations [^1][^2]

le TTL étant clairement définit comme une recomandation plus qu'une règle mène a certains problèmes. par exemple un résolveur peut choisir de ne pas suprimer ou mettre a jour une entrée de son cache malgré le TTL expiré. cela peut conduire a d'encienne ou obsolète informations en circulation. 

la documentation n'est également pas clair sur la marche a suivre pour mettre a jour les entrée du cache. en effet certain résolveur mettent a jour l'information dé l'expiration du TTL, alors que d'autres attendent qu'une requète arrive pour le RR en question.

le cache étant stoqué en mémoire et non sur dique, il est volatile et soumis a la limite de mémoire du support sur lequel le résolveur tourne. cela peut causer le résolveur a suprimer certaines entrée du cache pour faire de la place.

## Negative Cache [^3][^1]

jusque a présent nous avont vus le cas de réponces positives. c'est a dire que le résolver resoit bien la resource qu'il demande.

Il a été observé qu'une partie non négligeable du trafic DNS était des requètes résultant vers des réponces négatives. Pour éviter de demander plusieurs fois une ressource qui n'existe pas l'idée du 'Negative Cache' à été introduite. celui ci fonctionnerait comme le cache standard (a un ou deux détails près) et permet au résolveur de limiter les requète qu'il sait finiront par une erreur.

A la différance du cache vu plus haut, le résolveur ne ressoit pas de TTL avec une réponce négative. Il doit donc déterminer lui même le TTL aproprié. Pour cela, il utilise a la place le TTL minimum fournit dans la section d'authoritée de la réponse.

Dans le cas ou la réponce n'a pas de SOA l'erreur n'est pas mise en cache. Cela évite certaines boulces infinies de requètes.

## Bad Cache [^4]

l'arivée de DNSSEC a introduite de nouveaux problèmes dans les infrastructures. Ceux qui nous intéressent ici sont ceut du type administratif, comme une horloge désychronisée ou une zone no signée. Ce genre de problème ne pouvant être résolu par un simple revoit de requète, il est nécésaire de les différencier.

Pour cela le bad cache contient les requètes qui n'on pu être validée. Mais étant donée des utilisation néfaste possible de ces informations, l'implémentation du bad cache vient avec des règles notament concernant les attaques DoS:
- les TTL sont considèré non fiable et sont remplacé, de préférance par une petite valeur pour éviter de cacher les résulat d'une attaque.
- le resolveur ne doit pas réponde au requète a partir du bad cache sauf dans certains cas précis de configurations.


## Bibliographie

[^1]: P. Mockapetris, IETF, [RFC 1034 - DOMAIN NAMES - CONCEPTS AND FACILITIES](https://www.rfc-editor.org/rfc/rfc1034), November 1987
    
       **Résumé** : Standard définissant les concepts du DNS
       **Avis sur la ressource** : Il s'agit du texte de standardisation d'origine du DNS. Cependant, un certain nombre d'informations on été mises a jours dans des RFC plus récent.

[^2]: Robert Elz, Randy Bush, [RFC 2181 - Clarifications to the DNS Specification](https://www.rfc-editor.org/rfc/rfc2181), July 1997

       **Résumé** : Liste de clarification pour les RFC définissant le DNS
       ** Avis sur la ressource** : le TTL est décrit plus précisément que dans le RFC-1034

[^3]: M. Andrews, [RFC 2308 - Negative Caching of DNS Queries](https://www.rfc-editor.org/rfc/rfc2308), March 1998

       **Résumé** : Décrit en détail les procédure a suivre pour le cache de réponces négatives
       ** Avis sur la ressource** : exactement ce qu'il fallait ^^

[^4]: Roy Arends, Rob Austein, Matt Larson, Dan Massey, Scott Rose, [RFC 4035 - Protocol Modifications for the DNS Security Extensions](https://www.rfc-editor.org/rfc/rfc4035), March 2005

       **Résumé** : Partie de la suite de RFC décrivant DNSSEC, notament le bad cache
       ** Avis sur la ressource** : Pas toujours très clair