---
layout: default
title: Systemes_de_detection_d_intrusion.md
parent: Sécurité
---

# Intrusion Detection Systems

## Définition
Un système de détection d’intrusion détecte quand une personne n’étant pas censé avoir accès au système est présente sur ledit système. La détection peut avoir lieu à différents niveaux et la réaction de cas de détection d’intrus peut être passive ou active.
## Déploiement
Un IDS peut être déployé soit sur les hôtes d’un réseau, soit sur le réseau ou encore sur les deux. Ces deux IDS, IDS sur l’hôte (HIDS) ou sur le réseau (NIDS), diffèrent notamment sur la façon avec laquelle ils vont détecter un intrus.

NIDS : L’IDS sur réseau étudie les en-têtes des paquets réseaux et plus particulièrement les adresses IP source et destination, les ports source et destination et les drapeaux IP/TCP. Il étudie aussi la charge utile du paquet.

HIDS : L’IDS sur hôte, lui, étudie le trafic du réseau, comme il est déchiffré, les logs du système, la liste des processus, les appels système, le système de fichier, l’utilisation de la mémoire vive et du processeur.

## Réaction
Les IDS sont par définition passifs. Leur but est la détection. En cas de détection, ils alertent ou note la détection d’un intrus dans le log.
Cependant, ils existent aussi des systèmes actifs. Ils sont appelés systèmes de prévention d’intrusion (IPS). Ils peuvent simplement abandonner les paquets notés comme intrus ou bloquer l’adresse IP source dans le firewall, bloquer le compte de l’utilisateur ou empêcher une exécution.

## Utilisation malveillante et anomalie
L’utilisation malveillante est ce que l’IDS recherche à détecter alors que l’anomalie est la déviation de la norme. Pour que l’utilisation malveillante soit détectée, il faut définir ces utilisations malveillantes et comment les reconnaitre. Pour cela, on essaie de reconnaitre les signatures d’un type d’attaques. 

On peut diviser les résultats de détection en quatre catégories:

* Le vrai positif, quand il y a une intrusion et qu’elle est détectée.
* Le faux négatif, quand il y a une intrusion mais qu’elle n’est pas détectée.
* Le faux positif, quand il n’y a pas d’intrusion mais que l’IDS croit en avoir détectée une.
* Le vrai négatif, quand il n’y a pas d’intrusion et que rien n’est détecté.

En fonction de la sensibilité et de la spécificité de l’IDS, on va faire varier les résultats de détection. En augmentant la sensibilité, on augmente les chances de détecter les vrais positifs et en augmentant la spécificité, on diminue les chances de détecter de faux positifs.

On peut à nouveau diviser les IDS en deux catégories, ceux qui se basent sur la détection d’utilisation malveillante et ceux qui se basent sur la détection d’anomalies.
Ceux basés sur l’utilisation malveillante ne pourront détecter que les attaques connues alors que ceux basés sur les anomalies peuvent détecter des attaques inconnues.

Pour la détection d’anomalies, les HIDS regardent les traces d’appels système en créant un dictionnaire avec un paterne des appels systèmes utilisés et en détectant les appels systèmes inhabituels alors que les NIDS, qui ont le trafic réseau en clair, analysent la fréquence de caractères au sein de la charge utile du paquet.


## Evaluation de la précision
En utilisant des courbes ROC (Caractéristique d'exploitation du récepteur) permettent de comparer entre une sensibilité importante et une spécificité importante.
Ces courbes sont réalisées à partir de mesures sur un système. Il faut cependant faire attention aux taux de vrais positifs et de faux positifs à de l’erreur de taux de base.

### Source

* HERRMANN Dominik, “044-Network-Security-II”, Introduction to Security and Privacy, 2022 <https://www.uni-bamberg.de/fileadmin/psi/www.psi/teaching/Modul_PSI-IntroSP-B_V3_28.08.2020.pdf>
