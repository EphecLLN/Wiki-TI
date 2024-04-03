[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# Gestionnaire de paquets
## Qu'est-ce qu'un gestionnaire de paquets ?

Un gestionnaire de paquets est un outil qui permet à l'utilisateur d'installer, de désinstaller et de mettre à jour des logiciels via des paquets souvent prévus pour une distribution Linux.

## Pourquoi utiliser un gestionnaire de paquets ?

### Ils automatisent l'installation d'un logiciel.
Sans eux vous seriez obligé de télécharger le binaires d'une application, mettre ces derniers dans les bons dossiers, mettre à jour la variable d'environnement vous même (pour ne pas devoir donner le chemin absolu d'une application a chaque fois que vous voulez l'exécuter) et surtout ne pas devoir résoudre les dépendances d'une application vous-même.

### Ils assurent que le logiciel soit 100% compatible avec votre distribution.
Imaginez-vous installer un logiciel qui ne fonctionne pas totalement car il utilise des librairies trop veilles voir même trop récente et que cause a ce dernier de ne pas fonctionner totalement voire même de rendre votre system instable, eh bien tout ces sont éviter grace à des paquets maintenue pour être complètement compatible avec votre distribution. À noter que la plupart des gestionnaires de paquets proposent un serveur avec des versions "instables" des paquets qui permettent à ceux qu'ils le veulent à avoir accès aux dernières nouveautés au prix d'un system peut-être moins stable.

## Quels sont les principaux gestionnaires de paquets ?[1, 2, 3]

| Nom du gestionnaire de paquets| APT | DNF | YUM | ZYPPER | Pacman | SNAP | FLATPAK |
|---|---|---|---|---|---|---|---|
| **Distribution liée** | Debian/Ubuntu | Fedora/CentOS/RedHat Linux | RedHat Linux | OpenSUSE | Arch Linux | <p align="center">/<p> | <p align="center">/<p> |
| **Installeur** | dpkg | RPM | RPM | RPM | pacman | snap | flatpak |
| **Fromat** | .deb | .rpm | .rpm | .rpm | .tar.xx | <p align="center">/<p> | <p align="center">/<p> |

### À quoi servent les installeurs ?
Imaginons que, par exemple, vous soyez sur Debian, pour le cours de réseau vous avez besoin de packet tracer, pour ce faire vous allez sur le site de cisco et télécharger packet-tracer.deb, ce fichier est bien un paquet Debian mais il vous faut à présent l'installer, c'est là qu'entre en jeu l'application dpkg pour installer les paquets Debian.

Un gestionnaire de paquets sans installeur ne ferait que télécharger sur un serveur les paquets demandés par l'utilisateur sans les installers.

### Le cas Snap et Flatpak
Comme vous avez pu le voir Les gestionnaires de paquets Snap et Flatpak ne sont lié à aucune distribution, pourquoi cela ?
  
La raison est que grace à la technologie de containerisation, chaque application vit dans son monde, ces gestionnaires de paquets permettent aux applications de toujours fonctionner dans le même environement et donc de ne pas être dépendant de leur distribution.
  
Ces gestionnaires de paquets sont très clairement l'avenir car ils permettent au développeur de maintenir un seul paquet et de ne plus devoir résoudre les bugs spécifiques à une distribution. 
  
## Comment les utiliser ?
  
### APT[4]

* Mettre à jour les sources : 
  ```
  apt update
  ```
Permets de mettre à jour la liste des paquets disponible depuis le serveur source.
* Installer la dernière version des paquets installé sur le system : 
  ```
  apt upgrade
  ```
* Installer un nouveau paquet : 
  ```
  apt install *nom du paquet*
  ```
* Réinstaller un paquet : 
  ```
  apt reinstall *nom du paquet*
  ```
* Désinstaller un paquet : 
  ```
  apt remove *nom du paquet*
  ```
  * L'argument `--purge` est souvent ajouté pour ne laisser aucun fichier du paquet désinstallé, mais peut aussi être utiliser pour un paquet déjà désinstallé avec la commande :
  ```
  apt purge *nom du paquet*
  ```
  
* Rechercher un paquet en particulier : 
  ```
  apt search *nom du paquet*
  ```
* Lister les paquets installés : 
  
  ```
  apt list --installed
  ```
### DNF/YUM[5]

* Mettre à jour les sources : 
  ```
  dnf/yum check-update
  ```
* Installer la dernière version des paquets installée sur le system : 
  ```
  dnf/yum upgrade
  ```
* Installer un nouveau paquet : 
  ```
  dnf/yum install *nom du paquet*
  ```
* Réinstaller un paquet : 
  ```
  dnf/yum reinstall *nom du paquet*
  ```
* Désinstaller un paquet : 
  ```
  dnf/yum remove *nom du paquet*
  ```
  
* Rechercher un paquet en particulier : 
  ```
  dnf/yum search *nom du paquet*
  ```
* Lister les paquets installée : 
  
  ```
  dnf/yum list --installed
  ``` 
### ZYPPER[6]

* Mettre à jour les sources : 
  ```
  zypper refresh
  ```
* Installer la dernière version des paquets installée sur le system : 
  ```
  zypper update
  ```
* Installer un nouveau paquet : 
  ```
  zypper install *nom du paquet*
  ```
* Réinstaller un paquet : 
  ```
  zypper reinstall *nom du paquet*
  ```
* Désinstaller un paquet : 
  ```
  zypper remove *nom du paquet*
  ```
  
* Rechercher un paquet en particulier : 
  ```
  zypper search *nom du paquet*
  ```
* Lister les paquets installés : 
  
  ```
  zypper search --installed-only
  ``` 
  
### PACMAN[7]

* Mettre à jour les sources (non obligatoire) : 
  ```
  pacman -Syy
  ```
* Installer la dernière version des paquets installée sur le system : 
  ```
  pacman -Syu
  ```
* Installer ou réinstaller un paquet : 
  ```
  pacman -S *nom du paquet*
  ```
* Installer un paquet local :
  ```
  sudo pacman -U */chemin/vers/le/paquet*
  ```
* Désinstaller un paquet : 
  ```
  pacman -R *nom du paquet*
  ```
  
* Rechercher un paquet en particulier : 
  ```
  pacman -Ss *nom du paquet*
  ```
* Lister les paquets installés : 
  
  ```
  pacman -Qm
  ``` 
  
### SNAP[8]
  Si votre distribution n'inclut pas snap par défaut, il vous faudra l'installer vous-même en fonction de votre distribution.
  
  Voici le lien : https://snapcraft.io/docs/installing-snapd
  
* Mettre à jour les sources et installer la dernière version des paquets installée sur le system (automatique) : 
  ```
  snap refresh
  ```
* Installer un nouveau paquet : 
  ```
  snap install *nom du paquet*
  ```
  
* Désinstaller un paquet : 
  ```
  snap remove *nom du paquet*
  ```
  
* Rechercher un paquet en particulier : 
  ```
  snap find *nom du paquet*
  ```
* Lister les paquets installés : 
  
  ```
  snap list
  ``` 
  
### FLATPAK[9]
  Comme pour snap, si votre distribution n'inclut pas flatpak par défaut, il vous faudra l'installer vous-même en fonction de votre distribution.
  
  Voici le lien : https://flatpak.org/setup/
  
  L'un des plus gros avantages à souligner de flatpak est qu'il n'est pas obligatoire d'être `root` pour installer des paquets. 
  
* Mettre à jour les sources et installer la dernière version des paquets installée sur le system : 
  ```
  flatpak update
  ```
* Installer un nouveau paquet : 
  ```
  flatpak install *nom du paquet*
  ```
  
* Désinstaller un paquet : 
  ```
  flatpak uninstall *nom du paquet*
  ```
  
* Rechercher un paquet en particulier : 
  ```
  flatpak search *nom du paquet*
  ```
* Lister les paquets installés : 
  
  ```
  flatpak list --app
  ``` 
  
## Bibliographie
  
1. [Gestionnaire de paquets](https://fr.wikipedia.org/wiki/Gestionnaire_de_paquets), [auteurs](https://xtools.wmflabs.org/articleinfo/fr.wikipedia.org/Gestionnaire_de_paquets#top-editors), 4 août 2022, consulté le 19 août 2022.
    * Résumé : Explications de ce qu'est un gestionnaire de paquets.
    * Avis sur la source : Explique très bien ce qu'est un gestionnaire de paquets.
2. [Introduction to Flatpak](https://docs.flatpak.org/en/latest/introduction.html), [auteurs](https://github.com/flatpak/flatpak-docs/blob/master/docs/introduction.rst), 7 avril 2021, consulté le 19 août 2022.
    * Résumé : Explication sur ce qu'est flatpak.
    * Avis sur la source : Source offiel qui explique bien ce qu'est flatpak.
3. [Everything You Need to Know About Snap and Snap Store](https://www.makeuseof.com/everything-you-need-to-know-about-snap-and-snap-store/), Yash Wate, 21 juin 2021, consulté le 19 août 2022.
    * Résumé : Explication sur ce qu'est snap, comment il fonctionne, les avantages, les inconvénients et comment l'installer.
    * Avis de la source : Article très complet sans être trop technique.
4. [Ubuntu man apt](https://manpages.ubuntu.com/manpages/jammy/man8/apt.8.html), Canonical, 2019, consulté le 19 août 2022.
    * Résumé : Man de apt
    * Avis de la source : Documentation officiel ubuntu.
5. [Using the DNF software package manager](https://docs.fedoraproject.org/en-US/quick-docs/dnf/), [auteurs](https://pagure.io/fedora-docs/quick-docs/history/modules/ROOT/pages/dnf.adoc?identifier=master), octobre 2021, consulté le 19 août 2022.
    * Résumé : Documentation des commandes du gestionnaire de paquets DNF. 
    * Avis de la source : Documentation officiel fedora, simple mais efficace.
6. [OpenSUSE : Managing Software with Command Line Tools](https://documentation.suse.com/sles/12-SP4/html/SLES-all/cha-sw-cl.html#:~:text=Zypper%20is%20a%20command%20line,managing%20software%20from%20shell%20scripts.), [auteurs](https://github.com/SUSE/doc-sle/blob/maintenance/SLE12SP4/xml/sw_managing_commandline.xml), 1 août 2019, consulté le 19 août 2022.
    * Résumé : Documentation des commandes du gestionnaire de paquets zypper. 
    * Avis de la source : Documentation officiel openSUSE, très complète.
7. [pacman](https://wiki.archlinux.org/title/pacman), [auteurs](https://wiki.archlinux.org/index.php?title=Pacman&action=history), 1 août 2022, consulté le 19 août 2022.
    * Résumé : Documentation des commandes du gestionnaire de paquets pacman. 
    * Avis de la source : Documentation officiel Arch Linux, très complète.
8. [Getting started](https://snapcraft.io/docs/getting-started), Canonical, juin 2022, consulté le 19 août 2022.
    * Résumé : Documentation des commandes snap. 
    * Avis de la source : Documentation officiel snap, simple mais efficace.
9. [Using Flatpak](https://docs.flatpak.org/en/latest/using-flatpak.html), [auteurs](https://github.com/flatpak/flatpak-docs/blob/master/docs/using-flatpak.rst), 23 juillet 2021, consulté le 19 août 2022.
    * Résumé : Documentation des commandes flatpak. 
    * Avis de la source : Documentation officiel flatpak, très complète.
  
