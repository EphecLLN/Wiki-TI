---
layout: default
title: Kubernetes
parent: Réseaux
---

[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)

# Kubernetes

## Historique [1]

La méthode de déploiement de plusieurs applications à énormément évolué. Originellement, les applications étaient déployées sur le serveur physique. Cette solution a vite montré des limites de performances. Pour répondre à cette problématique, la virtualisation est apparue. Elle consiste à exécuter des applications dans des machines virtuelles (VMs) avec un système d'exploitation propre par-dessus l'OS hôte. Cela permet une structure évolutive et une sécurité plus importante par l'isolation des applications. Les VMs ont cependant encore un problème de ressource, chaque application nécessitant un Kernel. La conteneurisation est ensuite arrivée pour pallier à cela. Un conteneur est similaire à une VM dans son fonctionnement logique. la différence majeure est que les conteneurs sont exécutés sur l'OS hôte et non sur son propre OS. Cette avancée permet d'avoir des applications légères, une meilleure portabilité et une uniformité pour tous les environnements.

## Qu'est ce que Kubernetes ? [1] [2]

Kubernetes est un outil permettant la gestion de conteneurs. Il permet le déploiement et la restauration automatique de plusieurs applications sur plusieurs machines physiques disponibles dans le réseau Kubernetes. Il permet également la mise à l'échelle et facilite la mise à jour de ces applications. Toutes ses fonctionnalités ont pour objectif principal d'assurer une haute disponibilité et une gestion des ressources évolutive. 

Un environement Kubernetes, appelé Cluster, est composé de 1 ou plusieurs Node(s) ou nœud(s). Un nœud représente une machine physique du Cluster. Parmi les Nodes, au moins 1 doit servir de Master Node. Celui- où ceux-ci s'occupent, en plus des ressources applicatives déployées par le client, de gérer les composants nécessaires au fonctionnement du Cluster. Kubernetes propose un ensemble de ressources permettant la gestion des applications. L'unité de base de Kubernetes est un Pod. Il contient le ou les conteneurs nécessaires au fonctionnement de l'application, et se voit attribuer une adresse IP interne au Cluster.

## Quelques concepts Kubernetes [1] [2]

Comme expliqué ci-dessus, Kubernetes propose une multitude de concepts pour organiser une infrastructure d'applications. En voici une liste non exhaustive : 

1. Deployments

Les Deployments, où déploiements, sont des ressources permettant de contrôler un Replication Set. Ce dernier est une couche supplémentaire permettant la gestion plusieurs Pods basés sur une même image. Alors que les ReplicaSet s'occupent d'assurer la disponibilité d'un certain nombre de répliques d'une application, un déploiement permet de gérer la mise à jour des Pods en garantissant la disponibilité de l'application. Il permet aussi de garder un historique des versions de l'application et d'y revenir au cas où une mise à jour s'avère non fonctionnelle. 

2. Stateful Sets

Un Stateful Set est un contrôleur permettant d'assurer la persistance des données. Les Pods étant des unités non persistantes, et pouvant disparaitre à tout moment, il est parfois nécessaire de garder un certain nombre de données sur les serveurs. C'est notamment le cas pour des bases de données dont les données sont partagées sur un même volume.

3. Service

Un Service Kubernetes et une ressource permettant de faire le lien entre les différents Pods et l'extérieur. Il existe plusieurs types de services remplissant plusieurs objectifs différents. Les services de type NodePort ouvrent un port sur chaque Node du Cluster hébergeant l'application à laquelle le service est lié. Des services LoadBalancer vont eux utiliser un balanceur de charge externe pour rendre l'application disponible depuis l'extérieur. 

4. Volumes et Persistent Volumes

Les Volumes permettent de créer des ressources partagées pour tous les conteneurs dans un pod. La durée de vie d'un Volume est liée à celle du Pod. Les Persistent Volumes ont eu un cycle de vie indépendant du pod auquel il est lié. Cela permet d'assurer une persistance des données même si un pod vient à disparaitre. 

## Bibliographie

1. **Documentation Officielle Kubernetes**, Kubernetes, [What is Kubernetes?](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/), Dernière mise à jour le 4 avril 2022, consulté dernièrement le 7 mai 2022

    **Résumé** : Présentation officielle de Kubernetes
    **Avis sur la ressource** : Cette présentation explique clairement l'utilité de Kubernetes avec l'arrivée de la virtualisation ainsi que le fonctionnement de base de la technologie.
       
2. **Introduction aux concepts de Kubernetes**, Digital Ocean, [An introduction to Kubernetes](https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes), Justin Ellingwood, 1 octobre 2014 (dernière mise à jour le 2 mai 2018), consulté dernièrement le 7 mai 2022

    **Résumé** : Explication d'un bon nombre des concepts et composants de Kubernetes
    **Avis sur la ressource** : Ce site reprends beaucoup des concepts de Kubernetes avec une explication claire de chacun d'entre eux. 
