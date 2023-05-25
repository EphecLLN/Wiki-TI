---
layout: default
title: SSH
parent: Sécurité
---

# Article sur SSH
![download](https://github.com/HaAymar/Wiki-TI/assets/71372488/d67e13d1-c6d2-4202-958a-9b9b8283d461)

## SSH c'est quoi
SSH (Secure Shell) : Protocole de communication sécurisé nécessitant un échange de clés de chiffrement en début de connexion pour assurer une communication protégée entre un client et un serveur distant. Il permet l'authentification sécurisée de l'utilisateur, le chiffrement des données transmises et l'exécution de commandes à distance. Le SSH est largement utilisé pour l'accès distant à des systèmes informatiques, offrant une connexion fiable et sécurisée pour des tâches telles que l'administration système, le transfert de fichiers et le tunneling de ports.

## Protocoles SSH
Il existe 2 principales protocole SSH:
- SSH1  
- SSH2  

### Difference entre SSH1 et SSH2
- <b>SSH1</b> : C'est la première version du protocole SSH qui a été développée. Elle utilise principalement l'algorithme de chiffrement asymétrique RSA pour l'authentification et le chiffrement des données. Cependant, SSH1 a été largement remplacé par SSH2 en raison de problèmes de sécurité et de vulnérabilités connues. Il est recommandé d'éviter d'utiliser SSH1 et de privilégier SSH2.
- <b>SSH2</b> : C'est la version actuelle et largement utilisée du protocole SSH. Elle offre des améliorations significatives en termes de sécurité, de performances et de fonctionnalités.
 
### Fonction principale
- Authentification sécurisée
- Chiffrement des données 
- Transfert de fichiers sécurisé
- Exécution de commandes à distance 

## Utilisation du Sécure Shell
Le Secure Shell (SSH) est couramment utilisé pour accéder de manière sécurisée à des systèmes distants et exécuter des commandes à distance.
L'utilisation de SSH peut se faire avec un logiciel PuTTY ou sur le terminal macOs ou linux

### PuTTY 
![011500xnlg6t64fqgeggzz](https://github.com/HaAymar/Wiki-TI/assets/71372488/a37d7438-c048-4b4a-b085-317d4cf2ee23)

PuTTY est un client SSH gratuit et open-source largement utilisé pour se connecter à des serveurs distants en utilisant le protocole SSH. Il est principalement utilisé sur les systèmes Windows, mais il existe également des versions disponibles pour d'autres plateformes.

Utilisation de PuTTY s'effectue apres le telechargement de celui-ci [ici](https://www.putty.org/), apres il faudra selectionneé le protocole SSH, par defaut celui-ci est defini sur le port 22. 
Pour plus d'information vous pouvez consulter ce site en [cliquant ici](https://www.hostinger.fr/tutoriels/connexion-ssh-windows-putty)

![connect-config 399589842237e1bd21e24efe4d8052b5a5a77440462e3ab2f51a4c5019b74f19](https://github.com/HaAymar/Wiki-TI/assets/71372488/e1ffa45e-1abd-4610-bcfb-95f1a2fb9df6)

### Utilisation sur le terminal (Linux et MacOs)
Apres avoir ouvert le terminal, il existe plusieur commandes à effectuer pour se connecter en mode SSH :
- Connection SSH: 
 ```
 ssh username@hostname
```
 En fonction de la configuration du serveur distant, vous devrez vous authentifier. Vous pouvez utiliser un mot de passe ou une clé SSH. Si vous utilisez une clé SSH, votre clé publique devra se trouver dans le fichier `~/.ssh/authorized_keys`

### Comment se connecter avec la clé SSH
![61c1b963247368113bbeef17_Secure Shell work](https://github.com/HaAymar/Wiki-TI/assets/71372488/bf0ed866-8e78-43eb-ad93-4301dffc07b2)

Pour se connecter avec la clé ssh sur le serveur distant il faut creer la clé publique en suivant ces etapes:
- Creation d'une nouvelle clé sur le terminale:
 ```
ssh-keygen -t rsa
```
En suite on suit les étapes en creant le passphrase par défaut le fichier pour la clé privé et publique doit se trouvé dans les fichiers.
Pour la clé privé
 ```
.ssh/id_rsa
```
Pour la clé publique
 ```
.ssh/id_rsa.pub
```
Enfin il faut copier cette clé publique dans le serveur distant.

## Les avantages pour l'utilisations de SSH 
- Protection contre les attaques par force brute : SSH intègre des mécanismes de protection contre les attaques par force brute, ce qui signifie que si un attaquant essaie de deviner le mot de passe, le serveur SSH peut limiter le nombre de tentatives de connexion et bloquer automatiquement l'adresse IP source.

- Tunneling sécurisé : SSH permet la création de tunnels sécurisés, ce qui signifie que le trafic réseau entre le client et le serveur peut être acheminé à travers un canal chiffré. Cela est particulièrement utile lorsque vous devez accéder à des services distants (tels que des bases de données) via des réseaux non sécurisés.

- Administration à distance : SSH facilite l'administration à distance des serveurs. Vous pouvez vous connecter à un serveur distant via SSH et exécuter des commandes, modifier des fichiers de configuration, gérer des utilisateurs, etc. Cela permet aux administrateurs système de gérer leurs systèmes à distance de manière sécurisée.

- Portabilité : SSH est un protocole standard et est largement pris en charge sur différentes plateformes, y compris Linux, macOS et Windows. Cela signifie que vous pouvez utiliser SSH pour vous connecter à des serveurs distants depuis différentes machines, indépendamment du système d'exploitation utilisé.

## Bibliographie
1. https://www.hostinger.fr/tutoriels/connexion-ssh-windows-putty
2. https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/putty/
3. https://www.ibm.com/docs/fr/sia?topic=kbaula-enabling-rsa-key-based-authentication-unix-linux-operating-systems-2
4. https://www.putty.org/
5. https://www.paessler.com/it-explained/ssh
6. https://www.opportunites-digitales.com/ssh-tout-ce-que-vous-devez-savoir/


 
