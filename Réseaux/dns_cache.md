---
layout: default
title: Cache DNS
parent: Réseaux
---
# Cache DNS

## Pourquoi?

La nécessité de 'cache' le DNS devient vite apparente lorsque l'on réfléchit en termes du nombre de requêtes qu'un résolveur doit effectuer pour obtenir la réponse voulue par l'utilisateur. Au minimum, 2 à 3 requêtes, voire plus, sont nécessaires entre les différents serveurs d'autorité. Cela devient extrêmement redondant, par exemple, lorsque l'on considère le cas d'un utilisateur naviguant entre deux pages d'un même site ou entre deux services d'un même domaine. Par conséquent, afin de réduire l'impact sur le réseau et d'accélérer les temps de réponse, les RR (Resource Records) sont utilisés pour stocker temporairement les informations de résolution DNS.

## Fonctionnement [^1][^2]

### Structure du cache

Le cache est généralement stocké dans une structure de données, plus précisément un tableau de hachage. Dans un tableau de hachage, les noms de domaines ne sont pas stockés tels quels, mais sont converties grâce à un algorithme de hachage.  
L'avantage de cette approche est que la lecture des ressources se fait en temps constant plutôt que linéaire.

Les ressources stockées dans le cache comprennent les éléments suivants :
- Le hash du nom de domaine : il permet une récupération rapide en temps constant de la ressource associée.
- Le RR (Resource Record) reçu lors de la résolution : il contient les informations spécifiques associées au nom de domaine, telles que l'adresse IP correspondante.
- L'IP du serveur d'autorité : cette information est nécessaire pour mettre à jour les données du cache lorsque des changements sont détectés.
- Le TTL (Time To Live) : il s'agit d'un nombre entier exprimé en secondes, indiquant la durée de vie autorisée de la ressource dans le cache. Après expiration du TTL, la ressource est considérée comme périmée et doit être mise à jour.
- Le timecode : souvent exprimé en secondes également, il représente la date et l'heure auxquelles la ressource a été ajoutée au cache. Cela peut être utile pour effectuer des opérations de gestion et de suivi.

### Gestion du TTL

Le TTL n'étant qu'un entier représentant le nombre de secondes durant lesquelles la ressource est considérée valide, il ne se suffit pas à lui-même. Lors du cache d'une nouvelle entrée, un timecode est stocké avec le TTL et le reste des données.

Ensuite, lors de la résolution, le TTL et le timecode sont comparés. On soustrait le timecode au temps actuel, et si le résultat est supérieur au TTL, alors la ressource n'est plus considérée comme valide.

Si un TTL expire, deux comportements sont possibles :

- Le résolveur supprime l'entrée du cache. La requête suivante pour ce domaine déclenchera donc une résolution standard et une mise en cache des nouvelles données.

- Le résolveur tente de mettre à jour les données. Il envoie une requête au serveur d'autorité pour vérifier s'il existe une nouvelle version des données disponibles. Il met à jour l'entrée du cache et réinitialise le TTL.  
Cette approche permet d'éviter de refaire complètement la résolution DNS si les informations n'ont pas changé.

## Limitations [^1][^2]

Le TTL est considéré comme une recommandation plutôt qu'une règle stricte, ce qui peut entraîner certains problèmes. Un résolveur a la possibilité de ne pas supprimer ou mettre à jour une entrée de son cache même si le TTL a expiré. Cela peut entraîner la circulation d'informations obsolètes.  

La documentation n'est également pas claire sur la marche à suivre pour mettre à jour les entrées du cache. En effet, certains résolveurs mettent à jour l'information dès l'expiration du TTL, alors que d'autres attendent qu'une requête arrive pour le RR en question.

Le cache étant stocké en mémoire plutôt que sur un disque, il est volatile et soumis à la limite de mémoire du système sur lequel le résolveur s'exécute. Cela peut amener le résolveur à supprimer certaines entrées du cache pour faire de la place.

## Negative Cache [^3][^1]

Jusqu'à présent, nous avons examiné le cas des réponses positives, c'est-à-dire lorsque le résolveur reçoit avec succès la ressource demandée.

Il a été observé qu'une partie non négligeable du trafic DNS était constituée de requêtes aboutissant à des réponses négatives. Pour éviter de demander plusieurs fois une ressource qui n'existe pas, l'idée du "Negative Cache" a été introduite. Celui-ci fonctionne comme le cache standard, avec quelques détails différents, et permet au résolveur de limiter les requêtes qui aboutiront à une erreur.

À la différence du cache vu plus haut, le résolveur ne reçoit pas de TTL avec une réponse négative. Il doit donc déterminer lui-même le TTL approprié. Pour cela, il utilise à la place le TTL minimum fourni dans la section d'autorité de la réponse.

Dans le cas où la réponse ne contient pas de Start of Authority (SOA), l'erreur n'est pas mise en cache. Cela permet d'éviter certaines boucles infinies de requêtes.

## Bad Cache [^4]

L'arrivée de DNSSEC a introduit de nouveaux problèmes dans les infrastructures. Ceux qui nous intéressent ici sont ceux de type administratifs, tels qu'une horloge désynchronisée ou une zone non-signée. Ce genre de problème ne peut être résolu par un simple renvoi de requête, il est donc nécessaire de les différencier.

Pour cela, le bad cache contient les requêtes qui n'ont pas pu être validées. Cependant, étant donné les utilisations néfastes possibles de ces informations, l'implémentation du bad cache est accompagnée de règles, notamment concernant les attaques par déni de service (DoS) :

- Les TTL ne sont pas considérés comme fiables et sont remplacés, de préférence, par une petite valeur afin d'éviter de cacher les résultats d'une attaque.

- Le résolveur ne doit pas répondre aux requêtes à partir du bad cache, sauf dans certains cas précis de configurations.

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
