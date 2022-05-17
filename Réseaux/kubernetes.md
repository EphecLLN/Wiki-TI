---
layout: default
title: Kubernetes
parent: Réseaux
---

[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)

# Kubernetes

## Historique [1]

La méthode de déploiement de plusieurs applications a énormément évolué. Originellement, les applications étaient déployées sur le serveur physique. Cette solution a vite montré des limites de performances. 

Pour répondre à cette problématique, la virtualisation est apparue. Elle consiste à exécuter des applications dans des machines virtuelles (VMs) avec un système d'exploitation propre par-dessus l'OS hôte. Cela permet une structure évolutive et une sécurité plus importante par l'isolation des applications. 

Les VMs ont cependant encore un problème de ressource, chaque application nécessitant un Kernel. La conteneurisation est ensuite arrivée pour pallier à cela. Un conteneur est similaire à une VM dans son fonctionnement logique. La différence majeure est que les conteneurs sont exécutés sur l'OS hôte et non sur son propre OS. Cette avancée permet d'avoir des applications légères, une meilleure portabilité et une uniformité pour tous les environnements.

## Qu'est ce que Kubernetes ? [1] [2]

Kubernetes est un outil permettant la gestion de conteneurs. Il permet le déploiement et la restauration automatique de plusieurs applications sur plusieurs machines physiques disponibles dans le réseau Kubernetes. 
Il permet également la mise à l'échelle et facilite la mise à jour de ces applications. Toutes ses fonctionnalités ont pour objectif principal d'assurer une haute disponibilité et une gestion des ressources évolutive. 

Un environnement Kubernetes, appelé Cluster, est composé de 1 ou plusieurs Node(s) ou nœud(s). Un nœud représente une machine physique du Cluster. 
Parmi les Nodes, au moins 1 doit servir de Master Node. Celui- où ceux-ci s'occupent, en plus des ressources applicatives déployées par le client, de gérer les composants nécessaires au fonctionnement du Cluster. 

Kubernetes propose un ensemble de ressources permettant la gestion des applications. 
L'unité de base de Kubernetes est un Pod. Il contient le ou les conteneurs nécessaires au fonctionnement d'une application, et se voit attribuer une adresse IP interne au Cluster.

## Quelques concepts Kubernetes [1] [2]

Comme expliqué ci-dessus, Kubernetes propose une multitude de concepts pour organiser une infrastructure d'applications. En voici une liste non exhaustive : 

1. Deployments

Les Deployments, où déploiements, sont des ressources permettant de contrôler un Replication Set. Ce dernier est une couche supplémentaire permettant la gestion de plusieurs Pods basés sur une même image. 
Alors que les ReplicaSet s'occupent d'assurer la disponibilité d'un certain nombre de répliques d'une application, un déploiement permet en plus de gérer la mise à jour des Pods en garantissant la disponibilité de l'application. Il permet aussi de garder un historique des versions de l'application et d'y revenir au cas où une mise à jour s'avère non fonctionnelle. 

2. Service

Un Service Kubernetes est une ressource permettant de faire le lien entre les différents Pods et l'extérieur. Il existe plusieurs types de services remplissant plusieurs objectifs différents. Les services de type NodePort ouvrent un port sur chaque Node du Cluster hébergeant l'application auquel le service est lié. Des services LoadBalancer vont eux utiliser un balanceur de charge externe, avec son adresse IP propre, pour rendre l'application disponible depuis l'extérieur.

3. Volumes et Persistent Volumes

Les Volumes permettent de créer des ressources partagées pour tous les conteneurs dans un pod. La durée de vie d'un Volume est liée à celle du Pod utilisant ces données. Les Persistent Volumes ont eux un cycle de vie indépendant du/des pod(s) auquel/auxquels ils sont liés. Cela permet d'assurer une persistance des données même si un pod vient à disparaitre. 

4. Ingress

Un Ingress permet d'exposer des applications au travers de règles de routage. Cela permet d'avoir plusieurs applications accessibles sur la même adresse IP publique. Les Ingress s'occupent uniquement de gérer les accès depuis l'extérieur. Cette ressource nécessite cependant un contrôleur d'Ingress, qui est parfois intégré avec le fournisseur, sinon configuré en plus des autres ressources. 

## Exemple en application

Une entreprise possède 2 serveurs pour héberger des applications Web. Ces applications nécessitant une très haute disponibilité, une faible latence et pouvoir supporter beaucoup de requêtes, l'entreprise à décidé de mettre en ligne ces applications à l'aide de Kubernetes. Le schéma ci-dessous représente l'infrastructure Kubernetes de l'entreprise. 
Les 2 serveurs de l'entreprise font office de Master Node, ce qui permet d'assurer une haute disponibilité des services de gestion du Cluster Kubernetes.
Chaque application est mise en ligne à l'aide d'un Deployment. Chaque déploiement assure que 4 répliques de l'application soient en ligne dans toutes les circonstances. Les outils des Master Nodes permettent d'équilibrer la charge sur les différentes machines du Cluster. 

L'application App 2 nécessite une base de données à laquelle toutes les répliques doivent avoir accès. La base de données doit être déployée dans l'environnement Kubernetes à l'aide d'un Persistant Volume (PV). Chaque réplique d'App 2 y accède à l'aide du label indiqué lors de la création du PV, ou de son adresse IP interne au Cluster. Etant donné que c'est un persistant volume, les données ne disparaitrons si un Pod cesse de fonctionner. 

Pour permettre aux utilisateurs (User 1 et User 2) d'accéder à l'application App 2, cette dernière doit être exposée sur le réseau externe. Pour cela, 2 types de ressources Kubernetes sont créées dans le Cluster. La première est un Service. Chaque application possède son propre Service de type NodePort. Ce Service permet de rendre l'application disponible sur un port de l'adresse IP de chacun des Nodes. 
Acccèder à l'application au travers de l'adresse IP des Nodes n'est pas une bonne pratique. Cela réduit l'efficacité de l'équilibrage de charge et expose les adresses IP de tous les Nodes du Cluster. 
Pour pallier à cela, un Ingress est créé. L'Ingress récupère les Services des 2 applications et leur attribue une route. Lorsqu'un utilisateur désire accèder à l'application, celui-ci doit indiquer l'adresse IP (ou le nom de domaine si un [DNS est mis en place](https://kubernetes.io/fr/docs/concepts/services-networking/dns-pod-service/)) qui lui est attribué par le Ingress Controller et la route définie pour l'accès à l'application. 

![Infrastructure Kubernetes](https://user-images.githubusercontent.com/56077782/168284673-0240a527-f302-4835-8311-9c6a074cc591.png)

User 1 et User 2 désirent accéder à l'application App 2 en utilisant l'URL suivante: <adresse_IP>/app2. 
Les requêtes sont alors renvoyées par l'Ingress vers le Service de l'App 2. Celui-ci accède ensuite à l'un des Pods lié au Service de l'App 2. 
Les 2 utilisateurs ont alors accès à la même application avec les mêmes données, mais sur des répliques différentes.

## Bibliographie

1. **Documentation Officielle Kubernetes**, Kubernetes, [What is Kubernetes?](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/), Dernière mise à jour le 4 avril 2022, consulté dernièrement le 7 mai 2022

    **Résumé** : Présentation officielle de Kubernetes

    **Avis sur la ressource** : Cette présentation explique clairement l'utilité de Kubernetes avec l'arrivée de la virtualisation ainsi que le fonctionnement de base de la technologie.
       
2. **Introduction aux concepts de Kubernetes**, Digital Ocean, [An introduction to Kubernetes](https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes), Justin Ellingwood, 1 octobre 2014 (dernière mise à jour le 2 mai 2018), consulté dernièrement le 7 mai 2022

    **Résumé** : Explication d'un bon nombre des concepts et composants de Kubernetes
    
    **Avis sur la ressource** : Ce site reprends beaucoup des concepts de Kubernetes avec une explication claire de chacun d'entre eux. 
