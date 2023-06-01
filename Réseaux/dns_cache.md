---
layout: default
title: Cache DNS
parent: Réseaux
---
# Cache DNS

## Pourquoi?

La nécessitée de cache le DNS devient vite apparente quand on réfléchit en termes de nombre de requêtes qu'un résolveur doit faire pour obtenir la réponse voulue par l'utilisateur. Au minimum 2-3 et parfois bien plus de requêtes sont nécessaires entre les différents serveurs d'autorité. Ce qui devient fortement redondant si l'on prend l'exemple d'un utilisateur voyageant antre deux pages d'un même site ou deux service d'un même domaine. C'est donc pour alléger l'impact sur le réseau et accélérer les temps de réponse que les RR sont mis en cache a plusieurs étapes du processus.

## Fonctionnement [^1][^2]

### Structure du cache

le cache est généralement stockée dans une structure de données, plus particulièrement, un tableau de hash.
dans un tableau de hash les noms de domaines ne sont pas écrit tels quels mais sont converti grace a un algorithme de hachage. L'avantage de procéder comme ça est que la lecture des resources se fait en temps constant au lieu de linéaire.
les ressources stockées sont les suivantes:
- le hash du nom de domaine, pour la récupération en temps constant
- le RR reçu lors de la résolution
- l'IP du serveur d’autorité, nécessaire pour la mise a jour des données
- le TTL, nombre entier de seconde que la ressource est autorisée a vivre
- le timecode, souvent également en seconde, c'est la date et heure a laquelle la ressource a été rajoutée.

### Gestion du TTL

le TTL n'étant qu'un entier représentant le nombre de secondes durant laquelle la resource est considérée valide, il ne se suffit pas a lui même. lors du cache d'une nouvelle entrée, un timecode est stockée avec le TTL et le reste des données.

puis, lors de la résolution, le TTL et le timecode sont comparé. on soustrait le timecode au temps actuel, si le résultat est plus grand que le TTL alors la ressource n'est plus valide.

si un TTL expire, deux comportements sont possibles:
- le résolveur supprime l'entrée du cache. la requête suivante pour ce domaine causera donc une résolution standard et une remise en cache des nouvelle données.

- le résolveur tente de mettre à jour ces données. il envoie une requête au serveur d’autorité pour voir si une nouvelle version des données est disponible. il met à jour l'entrée du cache et reset le TTL.
cette approche permet d'éviter de complètement refaire la résolution DNS si les informations n'ont pas changé.

## Limitations [^1][^2]

le TTL étant clairement définit comme une recommandation plus qu'une règle mène a certains problèmes. par exemple un résolveur peut choisir de ne pas supprimer ou mettre à jour une entrée de son cache malgré le TTL expiré. cela peut conduire a des informations obsolètes en circulation. 

la documentation n'est également pas claire sur la marche a suivre pour mettre a jour les entrée du cache. en effet certain résolveurs mettent à jour l'information dès l'expiration du TTL, alors que d'autres attendent qu'une requête arrive pour le RR en question.

le cache étant stockée en mémoire et non sur disque, il est volatile et soumis à la limite de mémoire du support sur lequel le résolveur tourne. cela peut mener le résolveur à supprimer certaines entrées du cache pour faire de la place.

## Negative Cache [^3][^1]

jusqu'à présent nous avons vus le cas de réponses positives. c'est a dire que le résolveur reçoit bien la ressource qu'il demande.

Il a été observé qu'une partie non-négligeable du trafic DNS était des requêtes résultant vers des réponses négatives. Pour éviter de demander plusieurs fois une ressource qui n'existe pas l'idée du 'Negative Cache' à été introduite. celui-ci fonctionne comme le cache standard (à un ou deux détails près) et permet au résolveur de limiter les requête qui finiront par une erreur.

À la différence du cache vu plus haut, le résolveur ne reçoit pas de TTL avec une réponse négative. Il doit donc déterminer lui même le TTL approprié. Pour cela, il utilise à la place le TTL minimum fournit dans la section d’autorité de la réponse.

Dans le cas ou la réponse n'a pas de SOA, l'erreur n'est pas mise en cache. Cela évite certaines boucles infinies de requêtes.

## Bad Cache [^4]

l'arrivée de DNSSEC a introduit de nouveaux problèmes dans les infrastructures. Ceux qui nous intéressent ici sont ceux de type administratifs, comme une horloge désynchronisée ou une zone non-signée. Ce genre de problème ne pouvant être résolu par un simple renvois de requête, il est nécessaire de les différencier.

Pour cela le bad cache contient les requêtes qui n'ont pu être validée. Mais étant donnée des utilisations néfastes possibles de ces informations, l'implémentation du bad cache vient avec des règles notamment concernant les attaques DoS:
- les TTL sont considérés non fiable et sont remplacés, de préférence par une petite valeur pour éviter de cacher les résultats d'une attaque.
- le résolveur ne doit pas répondre aux requêtes à partir du bad cache sauf dans certains cas précis de configurations.


## Bibliographie

[^1]: P. Mockapetris, IETF, [RFC 1034 - DOMAIN NAMES - CONCEPTS AND FACILITIES](https://www.rfc-editor.org/rfc/rfc1034), November 1987
    
       **Résumé** : Standard définissant les concepts du DNS
       **Avis sur la ressource** : Il s'agit du texte de standardisation d'origine du DNS. Cependant, un certain nombre d'informations on été mises a jours dans des RFC plus récent.

[^2]: Robert Elz, Randy Bush, [RFC 2181 - Clarifications to the DNS Specification](https://www.rfc-editor.org/rfc/rfc2181), July 1997

       **Résumé** : Liste de clarification pour les RFC définissant le DNS
       ** Avis sur la ressource** : le TTL est décrit plus précisément que dans le RFC-1034

[^3]: M. Andrews, [RFC 2308 - Negative Caching of DNS Queries](https://www.rfc-editor.org/rfc/rfc2308), March 1998

       **Résumé** : Décrit en détail les procédure a suivre pour le cache de réponses négatives
       ** Avis sur la ressource** : exactement ce qu'il fallait ^^

[^4]: Roy Arends, Rob Austein, Matt Larson, Dan Massey, Scott Rose, [RFC 4035 - Protocol Modifications for the DNS Security Extensions](https://www.rfc-editor.org/rfc/rfc4035), March 2005

       **Résumé** : Partie de la suite de RFC décrivant DNSSEC, notamment le bad cache
       ** Avis sur la ressource** : Pas toujours très clair