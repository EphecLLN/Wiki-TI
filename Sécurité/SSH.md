---
layout: default
title: SSH
parent: Sécurité
---

# Article sur SSH
![download](https://github.com/HaAymar/Wiki-TI/assets/71372488/d67e13d1-c6d2-4202-958a-9b9b8283d461)

## SSH c'est quoi
SSH (Secure Shell) : Protocole de communication sécurisé qui nécessite l'échange de clés de cryptage en début de connexion pour garantir une communication sécurisée entre un client et un serveur à distance. Il permet d'authentifier l'utilisateur en toute sécurité, de chiffrer les données transmises et d'exécuter des commandes à distance. SSH est très utilisé pour accéder à distance à des systèmes informatiques, qui fourni une connexion fiable et sécurisée pour des tâches comme l'administration du système, le transfert de fichiers et le tunnel de port.

## Protocoles SSH
Il existe 2 principales protocole SSH:
- SSH1  
- SSH2  

### Difference entre SSH1 et SSH2
- <b>SSH1</b> : C'est la première version du protocole SSH qui a été développée. Elle utilise principalement l'algorithme de chiffrement asymétrique RSA pour l'authentification et le chiffrement des données. Cependant, SSH1 a été largement remplacé par SSH2 en raison de problèmes de sécurité et de vulnérabilités connues. Il est recommandé d'éviter d'utiliser SSH1 et de privilégier SSH2.
- <b>SSH2</b> : C'est la version actuelle et largement utilisée du protocole SSH. Elle offre des améliorations significatives en termes de sécurité, de performances et de fonctionnalités.
 
### Fonctions principales du SSH
- Authentification sécurisée
- Chiffrement des données 
- Transfert de fichiers sécurisé
- Exécution de commandes à distance 

## Utilisation du Secure Shell
Le Secure Shell (SSH) est couramment utilisé pour accéder de manière sécurisée à des systèmes distants et exécuter des commandes à distance.
L'utilisation de SSH peut se faire avec un logiciel PuTTY ou sur le terminal macOs ou linux

### PuTTY 
![011500xnlg6t64fqgeggzz](https://github.com/HaAymar/Wiki-TI/assets/71372488/a37d7438-c048-4b4a-b085-317d4cf2ee23)

PuTTY est un client SSH gratuit et open-source largement utilisé pour se connecter à des serveurs distants en utilisant le protocole SSH. Il est principalement utilisé sur les systèmes Windows, mais il existe également des versions disponibles pour d'autres plateformes.

L'utilisation de PuTTY s'effectue après avoir téléchargé celui-ci [ici](https://www.putty.org/), après l'installation, il faudra sélectionner le protocole SSH. Par défaut, celui-ci est défini sur le port 22. 
Pour plus d'information vous pouvez consulter ce site en [cliquant ici](https://www.hostinger.fr/tutoriels/connexion-ssh-windows-putty)

![connect-config 399589842237e1bd21e24efe4d8052b5a5a77440462e3ab2f51a4c5019b74f19](https://github.com/HaAymar/Wiki-TI/assets/71372488/e1ffa45e-1abd-4610-bcfb-95f1a2fb9df6)

### Utilisation sur le terminal (Linux et MacOs)
Après avoir ouvert le terminal, il existe plusieur commandes à effectuer pour se connecter en mode SSH :
- Connection SSH: 
 ```
 ssh username@hostname
```
 En fonction de la configuration du serveur distant, vous devrez vous authentifier. Vous pouvez utiliser un mot de passe ou une clé SSH. Si vous utilisez une clé SSH, votre clé publique devra se trouver dans le fichier `~/.ssh/authorized_keys`

### Comment se connecter avec la clé SSH
![61c1b963247368113bbeef17_Secure Shell work](https://github.com/HaAymar/Wiki-TI/assets/71372488/bf0ed866-8e78-43eb-ad93-4301dffc07b2)

Pour configurer l'authentification par clé SSH, vous devez prendre quelques étapes spécifiques. En premier lieu, vous avez besoin de générer une paire de clés publiques ou privées sur votre machine locale. Pour ce faire, il est possible d'utiliser l'utilitaire ssh-keygen en exécutant la commande suivante sur votre terminal:
- Creation d'une nouvelle clé sur le terminal:
 ```
ssh-keygen -t rsa
```
Ensuite, suivez les étapes. Vous devrez préciser un emplacement pour sauvegarder la clé privée. Par défaut, elle est généralement sauvegardée dans le répertoire ~/.ssh/ avec le nom "id_rsa". Cliquez sur "Entrée" pour accepter l'emplacement par défaut.

Vous devrez aussi définir un mot de passe(passphrase) afin de protéger la clé privée. Il est conseillé de choisir un mot de passe solide pour une sécurité optimale.
### Clé privée
C'est un fichier crucial pour l'authentification par clé SSH. Elle est générée sur votre machine locale et doit être gardée en sécurité. Voici l'emplacement de la clé privée:
 ```
cat ~/.ssh/id_rsa
```
### Clé publique
Elle est dérivée de la clé privée et doit être partagée avec les serveurs à distance auxquels vous voulez vous connecter. Voici comment obtenir la clé publique.

Une fois que vous avez généré la clé privée avec la commande précédente avec "ssh-keygen", vous pouvez afficher la clé publique en exécutant la commande suivante avec ".pub" qui signifie "publique"
 ```
cat ~/.ssh/id_rsa.pub
```
La clé publique sera affichée dans votre terminal. Vous pouvez la copier intégralement.

Après avoir généré votre clé privée et obtenu la clé publique, vous devez fournir la clé publique au serveur distant avec lequel vous souhaitez vous connecter. Généralement, le serveur SSH conserve la liste des clés publiques autorisées dans un fichier appelé "authorized_keys".

Voici comment ajouter votre clé publique au fichier "authorized_keys"
1. Connectez-vous au serveur distant via SSH 
```
ssh username@IP_ADDRESS
```
 Ensuite entrez votre mot de passe.</br>

2. Une fois connecté, exécutez les commandes suivantes pour ajouter votre clé publique à l'authentification par clé
```
mkdir -p ~/.ssh
echo "VOTRE_CLÉ_PUBLIQUE" >> ~/.ssh/authorized_keys
```

En suivant ces étapes, vous avez configuré SSH Key Authentication en utilisant les fichiers appropriés à l'emplacement correct.Ceci renforcera la sécurité de vos connexions SSH via une méthode d'authentification basée sur des clés asymétriques.
## Les avantages pour l'utilisations de SSH 
- Protection contre des attaques de force brutale : SSH intègre des mécanismes pour protéger contre les attaques de force brutale. c'est-à-dire que si un attaquant essaie de deviner le mot de passe, le serveur SSH limite le nombre de tentatives de connexion et bloque automatiquement l'adresse IP source.

- Tunneling sécurisé. : SSH permet de créer des tunnels sécurisés, ce qui signifie que le trafic réseau entre le client et le serveur peut être acheminé par un canal crypté. Ceci est particulièrement utile lorsque vous avez besoin d'accéder à des services à distance (comme des bases de données) par l'intermédiaire de réseaux non sécurisés.

- Administration à distance : SSH permet de gérer facilement les serveurs à distance. Vous pouvez vous connecter à un serveur à distance par SSH et exécuter des commandes, éditer des fichiers de configuration, gérer les utilisateurs, etc. Ceci permet aux administrateurs système de gérer leurs systèmes à distance de façon sécurisée.

- Portabilité: SSH est un protocole standard et est largement soutenu sur diverses plates-formes, y compris Linux, macOS et Windows. Ceci signifie que vous pouvez utiliser SSH pour vous connecter à des serveurs distants à partir de différentes machines, peu importe le système d'exploitation utilisé.

## Bibliographie
- [1] [Definition ssh](https://fr.wikipedia.org/wiki/Secure_Shell) Consulté le 24/05/2023
- [2] [Fonctions principales](https://www.paessler.com/fr/it-explained/ssh) Consulté le 24/05/2023
- [3] [Difference entre SSH1 et SSH2](https://waytolearnx.com/2017/12/difference-entre-ssh1-et-ssh2.html#:~:text=SSH2%20n'est%20plus%20un,chiffrer%20les%20flux%20de%20donn%C3%A9es) Consulté le 24/05/2023
- [4] [Se connecter avec PuTTy](https://kinsta.com/fr/blog/se-connecter-via-ssh/) Consulté le 25/05/2023
- [5] [Comment se connecter avec la clé SSH](https://lecrabeinfo.net/se-connecter-en-ssh-par-echange-de-cles-ssh.html) Consulté le 25/05/2023
- [6] [Les avantages pour l'utilisations de SSH](https://www.opportunites-digitales.com/ssh-tout-ce-que-vous-devez-savoir/) Consulté le 26/05/2023
- [7][Se connecter sur un serveur distant](https://www.hostinger.fr/tutoriels/generer-cle-ssh) Consulté le 02/06/2023

 
