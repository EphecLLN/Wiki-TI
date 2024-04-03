> ### _Carlier Louis_

# Article Wiki LoRaWAN - Carlier Louis 

![image](https://user-images.githubusercontent.com/63374771/186425838-bcc3a80a-eccc-464e-b0e1-96ca939edd88.png)

# Introduction



LoRaWAN (acronyme de "Long Range Wide-area network") est un protocole de télécommunication radio permettant les communications bas débit et ne nécessitant que très peu de consommation énergétique. 

Ce protocole est utilisé dans le cadre de l'internet des objets, c'est à dire l'interconnexion entre internet et des objets, des lieux ou des environnements physiques. Par exemple, il peut être utiliser dans le cadre de monitoring industriel ou encore pour des applications agricoles.

![image](https://user-images.githubusercontent.com/63374771/186428645-009de9d6-52d1-4146-a6f8-b5bf9ca59d41.png)

Source: https://knowhow.distrelec.com/fr/telecommunication/quest-ce-que-la-technologie-lora-lorawan/

<br>


Il est est fondé sur la technologie de modulation propriétaire LoRa et est créée en 2009 par la start-up grenobloise Cycléo. L'entreprise a ensuite été rachetée en 2012 pour 21 millions de dollars par le spécialiste américain des semi-conducteurs Semtech. 



# LoRa et LoRaWAN

LoRaWAN est un protocole tandis que LoRa fait référence à la couche physique permettant de connecter des capteurs ou objets nécessitant une longue autonomie de batterie à moindre coût.

LoRaWAN est la couche logicielle qui détermine la manière dont les appareils utilisent le matériel LoRa, il fait partie des technologies LPWAN (Low Power Wide Area Network) qui assurent une couverture longue portée tout en consommant peu de puissance. Les informations transportées peuvent circuler sur des distances plus longues que sur les réseaux de télécommunication traditionnels. Un objet connecté en LoRaWAN peut envoyer des données à une borne située à une distance d'1 kilomètre en zone urbaine et à plus de 20 kilomètres dans une zone rurale plane. 

![image](https://user-images.githubusercontent.com/63374771/186429090-4da121b3-f2ca-4e84-a057-9c6f36a9438b.png)

Source: https://knowhow.distrelec.com/fr/telecommunication/quest-ce-que-la-technologie-lora-lorawan/

<br>


Contrairement aux réseaux mobiles "classique" (4G, 5G), LoraWAN n'est pas taillé pour transporter de grande quantité de données, il n'est pas fait pour êtres utiliser pour satisfaire les besoins des smartphones par exemple. La technologie de modulation de fréquence utilisée ne permet de faire circuler que des petites quantité de données, comme par exemple avec les données émises par des capteurs à ultrasons ou de température par exemple. Le débit de transfert est compris entre 0,3 et 50 kilobits par seconde, ce qui est vraiment faible.

De plus, la spécification LoRaWAN est une norme qui permet une bonne intégration entre les dispositifs d’autres fabricants. Ce simple facteur est l’une des raisons pour lesquelles la technologie LoRa s’est vite accélérée dans le secteur de l’IoT.

<br>

Tableau comparatif des différentes technologies LPWAN: 

![image](https://user-images.githubusercontent.com/63374771/186467492-5856ac47-fdd2-4550-815f-6fa9df5dc959.png)

Source: http://cedric.cnam.fr/~bouzefra/cours/cours_Lora.pdf


# Fonctionnement et architecture de LoRaWAN 

L’architecture de LoRaWAN est divisé en quatre composants:
* End Nodes
* Passerelle
* Serveur réseau
* Serveur d’applications (dans le cloud)

![image](https://user-images.githubusercontent.com/63374771/186436742-6159b66e-ef81-455f-a7a3-ff71329d1d86.png)

Source: https://knowhow.distrelec.com/fr/telecommunication/quest-ce-que-la-technologie-lora-lorawan/

<br>


Les End Nodes (nœuds) sont situés à l’extrémité du réseau, ce sont des microcontrôleurs base consommation. Ils peuvent être déployé sur le terrain pendant de nombreuses années sans aucune maintenance. Ils sont équipés d’un émetteur LoRa pour pouvoir envoyer des paquets de données à la passerelle.

Les passerelles de LoRaWAN reçoivent les données des Ends Nodes. Les passerelles sont des transporteurs de paquets et elles permettent de faire la liaison entre les dispositifs et le réseau. Une passerelle transmet tous les paquets radio en liaison montante au serveur et inversement, sur la liaison descendante, la passerelle LoRa exécute les demandes de transmission provenant du serveur.

Le serveur réseau reçoit les données des passerelles et les transferts au serveur d'applications. Un grand nombre de fonctionnalités doivent être implémentées sur ce serveur, comme par exemple le filtrage des paquets en liaison montante en double ou encore l'exécution des mécanismes de gestion du réseau.

Le serveur d'applications permet d'interpréter les données reçues. C'est ce serveur qui est responsable du cryptage, du décryptage et du traitement des payloads de la couche application. De nombreuses application avec différents type de cryptage ou d'encodage peuvent être prise en compte par ce serveur afin d'assurer la sécurité des données et la qualité de la transmission.

# Couverture réseau de LoRaWAN

Chaque opérateur LoRaWAN dispose de son propre réseau et donc de sa propre carte de couverture. Il y a un total de 170 opérateurs dans le monde (chiffre de mai 2022) proposent un réseau LoRaWAN dans plus de 162 pays. En Belgique, le réseau est proposé par Proximus.

![image](https://user-images.githubusercontent.com/63374771/186466575-e27ad0e2-2fdc-4754-ba5c-3300da29cdc2.png)

Source: http://cedric.cnam.fr/~bouzefra/cours/cours_Lora.pdf

<br>



# Sécurité du réseau LoRaWAN

Une sécurité irréprochable est indispensable pour toute conception LPWAN. LoRaWAN utilise un cryptage AES 128 bits et comprend deux couches de sécurité indépendantes : une clé de session réseau (NwkSKey) et une clé de session d'application (AppSKey).

![image](https://user-images.githubusercontent.com/63374771/186444981-5792bc03-2e4f-4ccd-8bcb-cc5240dd1b6e.png)

Source: https://www.digikey.be/fr/articles/develop-lora-for-low-rate-long-range-iot-applications

<br>

_Le flux de données d'un dispositif de terminaison LoRa vers l'application inclut un processus de cryptage et de décryptage au début et à la fin de la chaîne, pour que seuls le capteur du nœud d'extrémité et l'application aient accès aux données en texte brut._

<br>

La couche de sécurité du réseau permet de garantir l'authenticité du nœud au sein du réseau tandis que au niveau de l'application, elle garantit l'interdiction d'accès aux données d'application de l'utilisateur final par l'opérateur réseau.

Logiquement, la clé applicative n’est connue que du fournisseur de l’application en question,  évitant ainsi qu’un tiers ne puisse consulter les données. Pour sa part, la clé réseau, est communiquée par l’opérateur du réseau aux fournisseurs d’applications autorisés.

Ces clés servent à sécuriser les données transmises par les équipements au travers du réseau mais elles sont également indispensables pour leur activation sur le réseau.

# Activation d’un équipement LoRaWAN

Avant de pouvoir effectuer une quelconque communication à travers un réseau LoRaWAN, les équipements doivent obtenir les clés de session, 2 méthodes d’activation sont possibles: OTAA (Over-The-Air Activation) ou APB (Activation By Personalization).

## Activation avec la méthode OTAA

Pour activer un équipement LoRaWAN avec cette méthode, l’équipement doit transmettre au réseau une demande d’accès ```join request```. Afin de réaliser cela, nous avons besoin de 3 paramètres:
1. Le DevEUI (id unique de l'équipement)
2. L'AppEU (id du fournisseur de l’application)
3. L'AppKey (clé AES 128 déterminée par le fournisseur de l’application)

L'équipement envoie la requête contenant ces 3 paramètres ainsi qu’un MIC calculé avec l'AppKey. Le serveur d'enregistrement va recevoir cette requête et vérifier le MIC grâce à la clé AppKey et si le serveur accepte la requête de l'équipement, la requête ```join accept``` est transmise en réponse à ce dernier.

Ensuite, les clés de session vont pouvoir être calculées à partir des données présentes dans la réponse du serveur.

![image](https://user-images.githubusercontent.com/63374771/186460781-fc8c12ce-b18f-4328-a89b-d8cb49bd9f4e.png)

Source: https://www.frugalprototype.com/technologie-lora-reseau-lorawan/


A chaque nouvelle session, ces clés sont renouvelées.

## Activation avec la méthode APB 

Avec cette méthode, les clés de session ainsi que l’adresse de l’équipement sont directement inscrits dans l’équipement LoRaWAN. De cette manière, l'envoi de requête n'est plus nécessaire avant de communiquer sur le réseau. 

Si on suit cette méthode, cela signifie que la communication a lieu sur un réseau spécifique car les clés de session sont connues d'avance. Contrairement à la méthode OTAA, ces clés sont statiques.

## Laquelle de ces méthodes utiliser ?

La méthode OTAA est plus sécurisé que la APB, cependant elle est plus complexe à mettre en place. 

Dans le cas d’un prototyoe ou pour une utilisation sur un réseau connu, la méthode APB est largement suffisante.

Pour un déploiement plus conséquent, il est conseillé d'utiliser la méthode OTAA afin d'assurer plus de sécruité. 


# Bibliographie

1) [LoRa / LoRaWAN](https://enless-wireless.com/fr/lora/) - Nom du site: **Enless Wireless** - Auteur: **/** - Date de visite: **20/08/2022** - Dernière date de modification: **/**

2) [LoRaWAN](https://fr.wikipedia.org/wiki/LoRaWAN) - Nom du site: **Wikipédia** - Auteur: **/** - Date de visite: **20/08/2022** - Dernière date de modification: **12/03/2022**

3) [Internet des objets](https://fr.wikipedia.org/wiki/Internet_des_objets) - Nom du site: **Wikipédia** - Auteur: **/** -  Date de visite: **20/08/2022** - Dernière date de modification: **14/06/2022**

4) [Lora](https://www.journaldunet.fr/web-tech/dictionnaire-de-l-iot/1197635-lora-comment-fonctionne-le-reseau-iot-20220509/) - Nom du site: **JDN** - Auteur: **/** - Date de visite: **20/08/2022** - Dernière date de modification: **/**

5) [M2M, IoT, LoRa, MQTT, …](https://www.atys-concept.com/blog-de-la-performance/articles-cybersecurite-et-reseaux/m2m-iot-lora-mqtt-comment-profiter-simplement-des-objets-connectes/) - Nom du site: **atys concept** - Auteur: **/** - Date de visite: **20/08/2022** - Dernière date de modification: **/**

6) [LoRa et LoRaWAN](https://knowhow.distrelec.com/fr/telecommunication/quest-ce-que-la-technologie-lora-lorawan/) - Nom du site: **knowhow.distrelec** -  Auteur: **Chris Rush** - Date de visite: **20/08/2022** - Dernière date de modification: **/**

7) [LoRaWAN](https://iotindustriel.com/technologies-solutions-iiot/sans-fil/lorawan-conception-et-mise-en-oeuvre-pour-liot/) - Nom du site: **IoT Industriel** -  Auteur: **Yasmine Aloui** - Date de visite: **20/08/2022** - Dernière date de modification: **/**

8) [Développez avec LoRa pour les applications IoT bas débit, longue portée](https://www.digikey.be/fr/articles/develop-lora-for-low-rate-long-range-iot-applications) - Nom du site: **Digi-Key** -  Auteur: **/** - Date de visite: **20/08/2022** - Dernière date de modification: **/**

9) [LoRaWAN](https://www.frugalprototype.com/technologie-lora-reseau-lorawan/) - Nom du site: **Frugal Prototype** -  Auteur: **Ali Benfattoum** - Date de visite: **20/08/2022** - Dernière date de modification: **/**

10) [Cours sur le Réseau LoRaWAN](http://cedric.cnam.fr/~bouzefra/cours/cours_Lora.pdf) - Auteur: **Samia Bouzefrane** - Date de visite: **20/08/2022**
 



