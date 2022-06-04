---
layout: default
title:Conteneurs Linux
parent: Réseaux
---
# Conteneurs Linux

## Un Conteneur qu'est-ce que c'est ? [^1]
Le mot conteneur (conteneur en anglais) désigne plusieurs objets différents en informatique, dans notre cas ce dernier désigne un 
environment d'exécution virtualiser. Un conteneur Linux (LXC) permet de faire tourner un programme sous linux sans devoir
tenir compte de l'environement

## Quelle est la difference entre un conteneur linux et une machine virtuel ? [^2]
Lorsque l'on virtualise un environment, on crée généralement une machine virtuelle. Pour cela le système d'exploitation
exécute un programme qui est chargé de virtualiser tous les composants de la machine virtuelle (VM). Ce dernier alloue des 
ressources à chaque composant de la VM, Il réserve une partie des cœurs du possesseur de l'hôte, une partie de la ram,
du disque dur ...  

La VM dé son démarrage aura accès à toutes ces ressources et elles lui seront entièrement réservée. De plus chaque VM 
possède son systeme d'exploitation complet.
Contrairement a une machine virtuelle un conteneur Linux lui n'a pas de système d'exploitation complet. Il partage son 
noyau (kernel) avec son hôte et les autres conteneurs. Cette difference majeurs permet au conteneur d'allouer et de libérer 
des ressources a chaud. En effet vu qu'il n'y a qu'un seul kernel en charge des ressources ces dernières peuvent simplement 
être transférée d'un conteneur a un autre comme s'il s'agissait de simple programme.


Les Conteneurs ne partagent cependant avec leur hôte que le kernel, tout le système de fichier est virtualisé, impossible pour un conteneur 
d'accéder au "file system" de l'hôte (Il est cependant possible de créer des volumes accessibles depuis le conteneur et l'hôte).

Contrairement à une Vm qui permet d'installer n'importe quel système d'exploitation (du moment que ce dernier est compatible
avec les composants virtuels), dans un conteneur Linux seul un système d'exploitation Linux peu être installé. 
Et cela dû au partage de kernel (Linux désigne le kernel, il existe beaucoup de système d'exploitation qui l'utilise).

## Pourquoi utiliser un conteneur Linux ? 
Grace a leur absence de noyaux dédier un conteneur linux sera beaucoup plus leger qu'une machine virtuelle.
Tout l'intérêt des conteneurs est reveler lorsque l'on désire exécuter un programme (conçu pour Linux) de manière fiable sur plusieurs 
machines différentes.
Le conteneur agira comme une capsule enfermant toutes les configurations nécessaires au programme. 
Ce dernier peut donc être exécuté sans erreurs peu importe l'environement

## Les technologies de conteneurs Linux [^3]

Les conteneurs Linux utilise une api du kernel linux cette dernière utilise les cgroups et d'autre technologie de 
cloisonnement de "name space" pour séparer le système hôte du conteneur.  

Plusieurs outils sont developer et maintenu par Canonical affin de facilité l'utilisation des conteneurs linux. 

### LXC
LXC est une interface permettant aux utilisateurs de gere simplement de conteneurs linux (elle est toujour maintenue par 
Canonical mais est obsolete)

### LXD
LXD est l'interface de nouvelle generation celle ci permet de gerer les conteneurs linux et les machine virtuelle de plus
elle utilise le format d'image linux. 

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