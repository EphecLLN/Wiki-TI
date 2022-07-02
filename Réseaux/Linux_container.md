---
layout: default
title:Conteneurs Linux
parent: Réseaux
---
# Conteneurs Linux

## Un Conteneur qu'est-ce que c'est ? [^1]
Le mot conteneur (container en anglais) désigne plusieurs objets différents en informatique, dans notre cas ce dernier désigne un 
environnement d'exécution virtualisé. Un conteneur Linux (LXC) permet de faire tourner un programme sous Linux sans devoir
tenir compte de l'environnement

## Quelle est la différence entre un conteneur Linux et une machine virtuel ? [^2]
Lorsque l'on virtualise un environment, on crée généralement une machine virtuelle. Pour cela le système d'exploitation
exécute un programme qui est chargé de virtualiser tous les composants de la machine virtuelle (VM). Ce dernier alloue des 
ressources à chaque composant de la VM, Il réserve une partie des cœurs du processeur de l'hôte, une partie de la ram,
du disque dur ...  

La VM dés son démarrage aura accès à toutes ces ressources et elles lui seront entièrement réservées. De plus chaque VM 
possède son systeme d'exploitation complet.
Contrairement à une machine virtuelle un conteneur Linux lui n'a pas de système d'exploitation complet. Il partage son 
noyau (kernel) avec son hôte et les autres conteneurs. Cette différence majeure permet au conteneur d'allouer et de libérer 
des ressources à chaud. En effet vu qu'il n'y a qu'un seul kernel en charge des ressources ces dernières peuvent simplement 
être transférées d'un conteneur à un autre comme s'il s'agissait de simples programmes.


Les Conteneurs ne partagent cependant avec leur hôte que le kernel, tout le système de fichiers est virtualisé, impossible pour un conteneur 
d'accéder au "file system" de l'hôte (Il est cependant possible de créer des volumes accessibles depuis le conteneur et l'hôte).

Contrairement à une VM qui permet d'installer n'importe quel système d'exploitation (du moment que ce dernier est compatible
avec les composants virtuels), dans un conteneur Linux seul un système d'exploitation Linux peu être installé. 
Cela est dû au partage de kernel (Linux désigne le kernel, il existe beaucoup de systèmes d'exploitation qui l'utilisent).

## Pourquoi utiliser un conteneur Linux ? 
Grâce à leur absence de noyaux dédié un conteneur Linux sera beaucoup plus léger qu'une machine virtuelle.
Tout l'intérêt des conteneurs est revelé lorsque l'on désire exécuter un programme (conçu pour Linux) de manière fiable sur plusieurs 
machines différentes.
Le conteneur agira comme une capsule enfermant toutes les configurations nécessaires au programme. 
Ce dernier peut donc être exécuté sans erreurs peu importe l'environnement

## Les technologies de conteneurs Linux [^3]

Les conteneurs Linux utilise une API du kernel linux cette dernière utilise les "cgroups" et d'autreS technologieS de 
cloisonnement de "name space" pour séparer le système hôte du conteneur.  

Plusieurs outils sont développés et maintenus par Canonical affin de faciliter l'utilisation des conteneurs Linux. 

### LXC
LXC est une interface permettant aux utilisateurs de gérer simplement des conteneurs Linux (elle est toujours maintenue par 
Canonical mais est obsolete)

### LXD
LXD est l'interface de nouvelle génération celle-ci permet de gérer les conteneurs Linux et les machines virtuelles; de plus
elle utilise le format d'image Linux. 

## Bibliographie

* [^1]: "[LXC — Wikipédia](https://fr.wikipedia.org/w/index.php?title=LXC&oldid=184410394)". Fr.Wikipedia.Org, 2008, Accessed 3 June 2022.

       **Résumé** : Page wikipedia de LXC 
       **Avis sur la ressource** : Page wikipedia tres complete . 

* [^2]: "[LXC Container Vs VM - Let's Discuss In Detail!](https://bobcares.com/blog/lxc-container-vs-vm/)". Bobcares, 2021. Accessed 3 June 2022.  
       
       **Résumé** : Les difference majeure entre les conteneur linux et les machine virtuele
       **Avis sur la ressource** : Article de journal informatique le journaliste n'est probablement pas un expert. 

* [^3]: "[Linux Containers](https://linuxcontainers.org/)". Linuxcontainers.Org, 2022. Accessed 3 June 2022.

       **Résumé** : Presente les differents technologie de conteur linux
       **Avis sur la ressource** : Site des developeur des technologie LXC, LXD, LXFS, DistroBuilder. 