---
layout: default
title: Délégation DNS
parent: Réseaux
---

# Délégation DNS

## Domaines, sous-domaines et zones

La délégation DNS est le mécanisme permettant à un domaine de ne pas devoir gérer l'ensemble des machines qui le compose.  Il peut décider de confier la gestion d'un sous-ensemble de ces machines à d'autres serveurs.  Ces sous-ensembles sont déterminés par la hiérarchie des noms DNS : il s'agit des sous-domaines.  

Ainsi, le domaine 'mondomaine.dom.' peut être découpé en sous-domaines, dont par exemple 'sousdom.mondomaine.dom.'.  Les machines qui appartient à 'mondomaine.dom.', mais à aucun des sous-domaines de ce dernier, constituent la **zone** 'mondomaine.dom.'.  Un serveur de nom doit obligatoirement gérer sa zone, mais peut déléguer ses sous-domaines à d'autres serveurs de noms.  

En effet, chaque sous-domaine est lui-même une zone, et doit donc posséder un serveur de nom (désigné par Name Server (NS) ou Authoritative Server (SOA)).  Dans notre exemple, il y aura donc (au minimum) une machine responsable pour la zone 'mondomaine.dom.', et une autre machine responsable de la zone 'sousdom.mondomaine.dom.'. 


## La délégation en pratique

La délégation consiste donc à ce qu'un serveur de nom d'un domaine redirige les requêtes concernant les sous-domaines au serveur de noms du sous-domaine concerné [1]. 

Pour cela, il doit posséder le nom du serveur de nom du sous-domaine dans son fichier de zone, sous la forme d'un record NS.  Néanmoins, cela n'est pas suffisant, puisque les clients auront besoin de l'adresse IP de ce serveur du sous-domaine.  

Par exemple, pour contacter 'www.sousdom.mondomaine.dom.', avoir le nom du serveur ns.sousdom.mondomaine.dom. n'est pas suffisant, il faut son adresse IP pour pouvoir lui envoyer une requête DNS.  Or, cette machine appartient à la zone "sousdom" dont elle est elle-même responsable!  

Pour pouvoir résoudre ce problème, il faut faire une exception au principe de répartition des machines par zone.  Il va falloir ajouter l'IP de la machine ns.sousdom.mondomaine.dom. au fichier de zone du NS de mondomaine.dom, alors que cette machine n'appartient théoriquement pas à cette zone.  

Cette adresse IP du serveur de nom d'un sous-domaine est encodée sous forme d'un record A.  Ce record est appelé **Glue Record** [1]. 

Dans le cas de notre exemple, la délégation de zone sera donc configurée en ajoutant les deux Resource Records suivants au fichier de zone du serveur de nom de 'mondomaine.dom.' : 

```
sousdom.mondomaine.dom.     IN  NS      ns.sousdom.mondomaine.dom.
ns.sousdom.mondomaine.dom   IN  A       1.2.3.4
```






## Bibliographie

* [1]: P. Mockapetris, IETF, [RFC 1034 - DOMAIN NAMES - CONCEPTS AND FACILITIES](https://datatracker.ietf.org/doc/html/rfc1034), Internet Draft, Nov. 1987
   - **Résumé** : Standard définissant les concepts du DNS
   - **Avis sur la ressource** : Il s'agit du texte de standardisation d'origine du DNS. Attention, il date de 1987 et possède de nombreuses mises à jour à consulter en parallèle. 
* [2]: **Slides du cours Admin I - Chapitre DNS**, V. Van den Schrieck, 2018, consulté le 10 janvier 2022
   - **Résumé** : Présentation de la hiérarchie DNS et des différents rôles des serveurs DNS
   - **Avis sur la ressource** : C'est un slideshow présentant les bases du DNS, qui gagnerait à être plus détaillé (schémas, texte, ...)

   
   

