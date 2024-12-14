# Le WLS2 :

## Qu'est-ce que c'est ?

Disponible depuis 2016, WSL qui signifie "Windows Subsystem for Linux", est comme son nom l'indique, 
un outil permettant de faire tourner un environnement GNU/Linux directement dans Windows. C'est un système bien plus pratique pour accéder à linux 
qu'un dual boot où de devoir créer une machine virtuelle.

Pour en profiter, il suffit d'activer la virtualisation sur son processeur dans le BIOS puis, après la simple commande PowerShell `wsl --install`, 
WSL est prêt à être utilisé. Il ne reste donc plus qu'à choisir parmi les distributions disponibles dans le Microsoft store pour pouvoir avoir accès 
à une machine linux profondément intégrée à Windows.

## Comment ça marche ?

Pour comprendre WSL il faut s'intéresser à ses différentes versions, car en effet le fonctionnement de WSL a changé depuis sa création.

A son lancement en 2016, WSL1 était une "Couche de compatibilité" pour faire tourner des fichiers binaires exécutables (ELF) nativement sur Windows 10. Aucune recompilation où portage des applications n'est nécessaire. WSL1 fournit un kernel compatible avec linux qui opère par dessus le kernel de Windows et qui va exécuter les fichiers binaires Linux. WSL1 va ensuite traduire les appels systèmes linux en appels systèmes Windows. Les applications Linux tournent dans les distributions qui fournissent toutes les dépendances et paquets nécessaires, un peu comme des conteneurs.

WSL1 a une approche semblable a Wine, qui est une couche de compatibilité bien connue permettant de faire tourner des exécutables Windows sur Linux en implémentant les appels systèmes et API des Windows dans des librairies.

En 2019, Microsoft annonce WSL2 qui possède un véritable kernel Linux basé sur la technologie Hyper-V. Ce qui se rapproche plus donc de la virtualisation plus "classique". Cette transition a permis un meilleur support des applications ainsi que de meilleures performances du file systeme.

Un kernel est une partie du système d’exploitation qui permet de gérer les ressources système et de faire le lien entre le matériel (hardware) et les logiciels (software)

Le fonctionnement de WSL2 est semblable à celui de Docker, le kernel est partagé entre toutes les instances de linux mais la mémoire et  les file systèmes sont différents. Cela permet de lancer une machine très rapidement car tout le système ne doit pas démarrer.

D’ailleur, en extractant le fichier .tar d’une distribution depuis une image Docker et en l’important dans WSL2, on peut faire tourner d’autres distributions que celles disponibles dans le microsoft store. Un des exemples les plus populaires de cette pratique est la distribution Arch linux.

## À quoi ça sert ?

WSL2 a de nombreuses utilisation très pratique qui améliore considérablement le confort d’utilisation de windows, pour le développement notamment. 

De nombreux développeurs préfèrent Linux a Windows pour les options et la liberté qu'il offre. Cependant, beaucoup préfèrent quand même la facilité et le confort de Windows au quotidien. Désormais, grâce à WSL2 il est possible de rester sous windows et de tout de même travailler dans la liberté qu’offre Linux.

L'intégration de WSL2 et de Windows permet également de faciliter la gestion des fichiers des machines Linux, en effet depuis l’explorateur de fichier WIndows, une section en bas baptisée linux contient l'entièreté du file systeme de chaque distribution installée et permet de facilement retrouver et éditer des fichiers depuis windows.

Mais cette possibilité fonctionne aussi dans les deux sens, depuis un terminal d’une des machines linux, on retrouve tout le file système de windows comme volume monté dans le répertoire /mtn/c/, cela permet donc de gérer tout son ordinateur windows grâce aux outils  puissants que sont les commandes Unix.

Un outil très pratique pour utiliser WSL2 c’est Vs code. Grâce à une extension fournie par microsoft, vs code permet de centraliser tous les outils pour utiliser WSL2, depuis une même fenêtre, on a accès à un ou plusieurs terminaux, a une arborescence de tous les file systeme de chaque distribution et à un éditeur de texte pour modifier des fichiers de config par exemple. 










## Bibliographie : 

https://docs.microsoft.com/en-us/windows/wsl/compare-versions

https://www.whitewaterfoundry.com/what-is-wsl#:~:text=WSL%20executes%20unmodified%20Linux%20ELF64,executes%20them%20at%20native%20speed.

https://docs.microsoft.com/en-us/windows/wsl/use-custom-distro

https://code.visualstudio.com/docs/remote/wsl

https://docs.microsoft.com/en-us/windows/wsl/faq

https://ling123labs.com/posts/WSL-files-in-Windows-and-vice-versa/
