# Z-Wave c'est quoi ?
![zwave](https://user-images.githubusercontent.com/49406566/186436922-07251c55-3185-4bc0-a917-738a3e38d896.png)

Créé en 1999, Z-Wave est un protocole de communication radio sécurisé et dédié à une application domotique. Ainsi, ce protocole permet à différents appareils électroniques, pouvant être de marques différentes ou non, d'interagir entre eux au sein d'un domicile via une bande de fréquences isolée du wifi, de la 4G ou encore des ondes radios classiques. <br/>
Il est optimisé pour des échanges à faible bande passante à contrario du Wi-Fi qui est quant à lui optimisé pour des échange à haut débit. Ainsi, les appareils reliés entre eux via ce protocole pourront se faire passer une commande donnée par l'utilisateur dans un périmètre d'environ 50 mètres, en fonction de la localisation intérieure ou extérieure de l'installation.<br/>
En terme d'exemples concrets, nous pourrions citer : 
* Si la porte d'entrée s'ouvre entre 10 et 18h, envoyer une notification sur le téléphone du propriétaire,
* Si la luminosité ambiante est trop faible, allumer les lumières de la pièce,
* Si une personne reconnue par le système entre dans la maison, lui lire un message personnalisé ou un rapport concernant les différentes notifications/alertes reçues durant son absence.

Un des principaux avantages de Z-Wave est qu'il est très simple de créer une routine/scénario simplement et ne nécessite aucunement des connaissances dans le domaine de la programmation. Néanmoins, il est bien évidemment possible de créer des scénarios plus complexes en s'appuyant sur des langages comme Python, Bash, ...

# Quels avantages et spécificités ?
Z-Wave fonctionne à très faible consommation, ce qui permet d'économiser beaucoup d'énergie. Effectivement, les différents services resteront en état de repos tant qu'ils ne seront pas sollicités avant de se réveiller lors d'une interaction avec eux, ce qui augmente grandement la durée de vie de la batterie.<br/>
Le protocole Z-Wave, à contrario du Wifi et d'un réseau 4G, travaille sur une bande de fréquences, de 868MHz en Europe, totalement isolée du reste du réseau. <br/>
Il travaille également en réseau maillé, ce qui permet de facilement mettre en place une structure non hiérarchiques entre les différents nœuds du réseau. Ce réseau maillé représente un réel avantage au niveau des échanges entre les différents appareils car il permet de facilement pouvoir venir ajouter un retirer un nœud de ce réseau sans devoir retravailler sur l'entièreté de celui-ci. Également, ce réseau maillé permet une alimentation facile entre tous les composants du réseau, de nombreuses routes sont créées à travers ce même réseau tout en augmentant le rayon de ce dernier. Enfin, dernier avantage de ce réseau maillé, lorsqu'un service ne sera plus disponible, les services avoisinant serviront de relais pour transmettre l'instruction aux autres et ainsi ne pas arrêter cette dernière à la moindre erreur d'un service.<br/>
Également, Z-Wave propose une comptabilité complète entre n'importe quel type de matériel, de n'importe quelle marque, tout est fait pour que l'inter compatibilité soit assurée par ce protocole. Évidemment, les services doivent comporter le logo Z-Wave pour tout de même fonctionner dans le réseau du même nom.<br/>
Un autre avantage de ce protocole est la sécurité qu'il offre aux communications, pour ce faire, il se base sur un chiffrement asymétrique AES dont je reparlerai plus tard.

# Quels sont les désavantages de ce protocole ?
Afin d'assurer la compatibilité de différents appareils les uns avec les autres, ces derniers doivent être soumis à la certification Z-Wave. Une fois cette certification acquise, le logo Z-Wave apparait sur le produit et il est ainsi certifié compatible avec les autres éléments Z-Wave. Néanmoins, tout cette certification a un coût qui se répercute incontestablement sur l'acheteur qui se verra son prix d'achat assez important. Également, les producteurs désirant s'allier à l'environnement Z-Wave doivent passer par cette certification qui leur impose un coût certain. En réalité, ce premier désavantage n'en est pas réellement un pour quelqu'un qui veut s'assurer de la compatibilité de ses devices et de la sécurité de ces derniers.<br/>
Il est également important de se renseigner sur la compatibilité de produits issus de produits différents, effectivement la fréquence de ces derniers varie en fonction des pays où le produit est fabriqué, voici une petite sélection de pays avec lur fréquence Z-Wave entourée : 

![image](https://user-images.githubusercontent.com/49406566/186449411-1f2eb449-0a9e-4da2-af33-5fcd39d9e531.png)

Il est donc primordial de s'assurer de la bonne compatibilité des produits si ceux-ci proviennent de pays étrangers (en Europe, la majorité des pays sont sur la même fréquence (CEPT dans l'image ci-dessus, 868.4 MHz).<br/>
Le fait d'avoir également une plage de fréquence isolée du Wifi et du réseau 4G représente d'un coté un avantage mais également un inconvénient car le débit maximal de communication s'élève à 100kB/s, ce qui peut empêcher certains services de fonctionner comme des sonnettes connectées. <br/>
Voici la carte des pays dans lesquels nous retrouvons aujourd'hui des produits équipé de Z-Wave.
![image](https://user-images.githubusercontent.com/49406566/186467245-7640a855-00fa-4281-9fb9-ac1e06ac89a0.png)


# Comment fonctionne ce protocole
Tous les services Z-Wave sont connectés à un "Hub" général qui va gérer l'ensemble du réseau.<br/>
Le protocole Z-Wave se base sur le standard ITU-T G.9959 qui définit les spécifications des couches physique et MAC pour les émetteurs-récepteurs de radiocommunication numériques à bande étroite et à courte portée et ce, sans donner la liste des fréquences utilisées par les dispositifs. <br/>
Au dessus de ces deux couches, nous retrouvons une couche applicative. Cette couche a pour fonction de présenter les différentes informations utilisées pour le contrôle et la gestion des modules. Voici un schéma présentant la liaison entre ces différentes couches.

![image](https://user-images.githubusercontent.com/49406566/186417100-6a7cd21c-101f-4428-bc64-12c2e1950546.png)

Dans la couche applicative : 
* Le "header" permet ici d'indiquer de quelle type de trames il s'agit,
* Le "cmd" class permet d'indiquer de quel type de requête il s'agit,
* Le "command" permet d'indiquer une commande à exécuter,
* Les "parameters" sont différents paramètres à prendre en compte ou à utiliser pour la requête.

Dans la couche MAC : 
* Le HomeID indique l'émetteur et la destinataire d'un message. Si la valeur de cet HomeID est à "0xFF", la trame sera de type broadcast et le message sera donc envoyé à chaque équipement du réseau.

Toutes les communications sont organisées en classes de commande. Ces classes sont utilisées afin de réaliser des actions précises, ces actions sont bien évidemment limitée aux fonctionnalités offertes par les équipements du réseau. Chaque classe peut ensuite définir une ou plusieurs commandes, avec différents paramètres. Par exemple, la classe COMMAND_CLASS_SWITCH_BINARY permet de spécifier différents types de commandes :
* Les commandes SET, qui permettent de définir un état,
* Les commandes GET, qui permettent de récupérer un état,
* Les commandes REPORT, qui permettent de stocker un état en paramètre.

Si l’on souhaite allumer une lampe, on pourra donc utiliser la commande SET pour définir le nouvel état, en passant en paramètre la valeur de cet état (0xFF pour 1/allumée, 0x00 pour 0/éteinte). Si le contrôleur domotique souhaite connaître l’état du périphérique, il utilisera la même classe, mais avec la commande GET. La lampe enverra ensuite une commande REPORT pour signaler son état en paramètre.<br/>

Afin que le contrôleur soit en mesure de savoir s’il doit utiliser la classe de sécurité pour l’envoi des messages, une trame NIF (Node Information Frame) est envoyée lors de l’inclusion d’un module au réseau Z-Wave. Si vous utilisez OpenZWave, la liste des classes reconnues et sécurisées pour les différents périphériques est stockée dans le fichier de configuration zwcfg_[HomeID].xml.

Afin de chiffrer les communications, une clé dédiée au réseau Z-Wave cible est tout d’abord échangée entre le contrôleur et le périphérique lors de la phase d’inclusion (représentée en bleu dans la figure 3) :

![image](https://user-images.githubusercontent.com/49406566/186463166-930b0f13-3dee-47bc-a932-7aa13acf5542.png)


# Utilités de la couche applicative
Comme expliqué précédemment, la couche applicative du protocole Z-Wave permet de fournir des informations sur le type de classe de commande, la fonction utilisée ainsi qu'une liste de paramètres.<br/>
La couche applicative permet également d'assurer une communication chiffrée en AES 128 bits (Advanced Encryption Standard ou AES ,est un algorithme de chiffrement symétrique. Il s'agit du nouveau standard de chiffrement pour les organisations du gouvernement des États-Unis. Il a été approuvé par la NSA (National Security Agency) dans sa suite B1 des algorithmes cryptographiques. Il est actuellement le plus utilisé et le plus sûr). Pour ce faire, la couche applicative va utiliser la classe de commande "COMMAND_CLASS_SECURITY" qui permet d'encapsuler les données de la couche comme dans l'image ci-dessous : 

![image](https://user-images.githubusercontent.com/49406566/186442112-37e8a875-a6cc-4036-a6a3-df947ab40149.png)

La couche applicative est ainsi primordiales pour le bon envoi des requêtes ainsi que la sécurisation de ces dernières.

# Bibliographie
1. [Journal du Net](https://www.journaldunet.fr/web-tech/dictionnaire-de-l-iot/1440712-z-wave-caracteristiques-et-evolution-du-reseau/)
2. [Planète Domotique](https://www.planete-domotique.com/blog/2019/11/15/top-10-z-wave/)
3. [Wikipédia](https://fr.wikipedia.org/wiki/Z-Wave)
4. [Homey](https://homey.app/fr-be/wiki/quest-ce-que-z-wave/)
5. [Diamond Connect](https://connect.ed-diamond.com/MISC/misc-082/la-securite-du-protocole-z-wave)
6. [Clemovernet](https://clemovernet.wordpress.com/2015/08/05/z-wav-debuter-controleur-en-cs/)
7. [Vidéo Youtube](https://www.youtube.com/watch?v=k2qOh1O5tug&ab_channel=EverythingSmartHome)
8. [Silicon Labs](https://www.silabs.com/wireless/z-wave/global-regions)

