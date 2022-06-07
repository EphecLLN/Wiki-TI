[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# Article Wiki MQTT - Momin Maxime 2TL2


## Introduction [1][2]

MQTT est un protocole basé sur TCP-IP au niveau de la couche applicative. Celui-ci appartient aux protocoles de messagerie dit de type **publish-subscribe**.
Il a vu le jour avec une première version en 1999 et est né de la collaboration de Andy Stanford-Clark (IBM¹) et Arlen Nipper (Eurotech²). 
MQTT a été conçu dans le but d'étre utilisé dans les zones où la bande passante est tout d'abord limitée. Ensuite, ces principaux objectifs étaient d'être efficace en therme de bande passante, d'être léger et également de posséder un faible dépense en énergie.

Ce protocole, possédant une moindre surcharge en therme d'en-têtes que d'autres protocoles comme HTTP³, excelle au niveau du transfert de données comparés à d'autres protocoles de communication. Un point important à spécifier est que celui-ci est décrit comme être facile à mettre en oeuvre coté client et convient très bien à des environnements possédants des ressources limitées. Etant donné que ce critère était également l'un des objectifs initiaux du protocole, il est important de le souligner.

Il n'est pas rare de voir MQTT qualifié de protocole de file d'attente. Cette qualification est trompeuse étant donnée que MQTT ne présente pas de file d'attente comparable aux différentes solutions plus traditionnelles de communication. 

 
*IBM¹: Société multinationale américaine présent dans le domaine informatique*  
*Eurotech²: Société italienne dans le secteur de l'informatique*  
*HTTP³: Protocole de communication client-serveur*  


## Qu'est-ce qu'un protocole "publish-subscribe"? [1][3][4]

Pour comprendre MQTT, il est impératif de comprendre ce qu'est un protocole que l'on qualifie de "publish-subscribe". 

Tout d'abord, il faut savoir que ce protocole utilise un mécanisme particulier pour transférer les données et est composé de deux types d'acteurs et d'un domaine de donnée:

- Le domaine de donnée est appelé le **courtier** ou **broker**.
- Le premier type de client est **l'éditeur** ou plus communément qualifié le **publisher**.  
*-> Les clients publiant des données sur le courtier sont qualifiés d'éditeurs.*
- Le second type sont les **abonnés** ou **subscriber**.  
*-> Les clients récupèrant les informations publiées sur le courtier sont qualifiés d'abonnés.*

Un éditeur n'envoie des messages/données pour, à priori, aucun distanateur bien spécifique. A la place, les messages sont envoyé à une catégorie bien spécifique présente sur un courtier. *Il peut, bien entendu, il y avoir une multitude de catégories différentes présentes sur le courtiers.* 

Mais comment recevoir les données dans ces circonstances vous demandez-vous ? Et bien tout simplement par principe d'abonnement. Un dabonné souhaitant recevoir des données s'abonnera à la ou les catégories souhaitées et recevra donc les données présentes dans celles-ci.

Bien entendu, un éditeur ne peut donc pas savoir si ses messages envoyés au courtier seront lus ou non par un abonné quelconque. Idem pour les abonnés qui pourront s'abonner aux catégories souhaitées mais n'aurons pas connaissance de l'existance d'un éditeur lié à cette catégorie. Seul le courtier est connu des différents éditeurs et abonnés.

![](https://github.com/M-Momin/Wiki-TI/blob/main/Assets/Images/MQTT_publish-subscribe_sh%C3%A9ma.png)

*Shéma réalisé via draw.io*

### Dissociation ? [1][8]

Un des élément fondamentale spécifique à ce modèle est la dissociation complète entre les éditeurs et les abonnés et ce sur plusieurs plans.  

- La **dissociation spatiale**: Les acteurs n'ont pas besoin de connaitre les informations de l'autre tel que l'adresse IP ou encore le port.
- La **dissociation temporelle**: L'éditeur ainsi que l'abonné ne doivent pas nécessairement fonctionner au même instant pour communiquer. 
- La **dissociation de synchronisation**: Se traduit par le fait que l'envoie ou la réception d'un élèment n'interrompt pas les opérations s'exécutant sur les deux acteurs.

### Le filtrage [1][5]

Il est également interressant de savoir, qu'avec l'aide d'un filtrage bien défini, les messages peuvent être reçus seulement par certains destanataires bien spécifiques.

Il existe 4 types de filtrage différents dans le cas du modèle "pub-sub":

1. **Filtrage par sujet**:
Les messages envoyés sont reçus et stockés dans des catégories logiques plus communément appelés "sujets". Les différents abonnés d'un système, qui sont abonnés aux différents sujets visés, auront accès à toutes les informations envoyés et stockés dans ces catégories logiques spécifiques.

2. **Filtrage basé sur le contenu**:
Ici, les messages sont filtrés selon leurs attributs et/ou leur contenu. Un abonné recevra donc uniquement les messages correspondant aux différentes contraintes qu'il a spécifié. Il est important de préciser que cette méthode possède un inconvénient qui est que le contenu doit être connu avant toutes choses par l'abonné ainsi que par l'émetteur pour fonctionner correctement. 

3. **Filtrage par type**:
Dans ce cas-ci, le filtrage se fait en fonction de l'évènement ou du type. Il est coutume d'utiliser cette méthode dans les languages qualifiés d'orientés objet.

4. **Filtrage hybride**
Dans certaines situations, la combinaison du filtrage par sujet et de celui basé sur le contenu est d'application. Dans un premier temps, des messages sont publiés dans des sujets spécifiques (Filtrage par sujet). Par la suite, les abonnés récupèrent des messages basés sur le contenu de différents sujets correspondant à leurs spécifications ultérieures (Filtrage basé sur le contenu).

### Points importants à savoir [1]

Dans le modèle pub/sub, il est important de connaitre, au préalable, les différents détails et aspects qui définissent la structuration des données établies (Différents contenus, attributs, sujets ...). Comme nous avons pu le constater dans le cas du filtrage basé sur le contenu, les 2 acteurs doivent connaitre les différents sujets avant quoi que ce soit.
Ensuite, il faut bien comprendre que les messages envoyés par un éditeur ne seront peut-être jamais reçus/lus par un abonné.


## MQTT dans tout ça ?

### MQTT - Modèle pub/sub [1][2][8]

MQTT découpe l'espace en 2 parties distinctes composées de l'émetteur (publisher) et de l'abonné (subscriber). Ceux-ci doivent uniquement avoir pris connaissance de l'adresse ip et du port du courtier afin de pouvoir, dans le cas de l'émetteur, envoyer des messages et, dans le cas de l'abonné, s'abonner afin d'avoir la possibilité de recevoir des messages spécifiques. MQTT se dissocie donc d'une certaine manière spatialement.

Généralement, l'utilisation de MQTT concerne une conversation quasi instantannée entre l'éditeur et l'abonné. Quand ce n'est pas le cas, une solution de repli existe car MQTT se dissocie temporellement. Le courtier peut donc stockés les informations afin qu'elles soient accessibles plus tard pour les distanataires qui ne sont pas actuellement connectés. Pour que cela se fasse, il faut respecter deux conditions:
1. L'abonné s'est déja connecté une fois avec une session persistante.
2. L'abonné possède sur la catégorie spécifique une QoS¹ de 1 ou 2.

MQTT peut également faire de la dissocation de synchronisation. Une bonne partie des bibliothèques clientes fonctionnent, à ce jour, de manière asynchrone à l'aide de fonction de rappels. Cela signifie que MQTT n'empêchera aucune tâches s'exécutant pendant la publication ou l'attente d'un message.

MQTT fonctionne avec un filtrage par sujet. Chaque message émis par un éditeur au courtier contient donc un sujet. Le courtier utilise ce sujet afin de vérifier si oui ou non un abonné peut recevoir ce message spécifique.

*QoS¹: la qualité de service décrit la capacité de transmettre des informations dans de bonnes et certaines conditions (les QoS concernant MQTT seront vues plus tard dans l'article.* 

### MQTT - Qualités de services [6][7]

Il existe trois types différents de qualités de services pour le protocole MQTT. A savoir que chaque connexion au courtier est spécifiée par une valeur allant de 0 à 2. 

1. QoS = 0  
**Au plus une fois** qui signifie que le message sera envoyé uniquement envoyé une fois à l'abonné sans accusé de réception. Si l'abonné n'est pas en ligne ou que le message se perd, il ne sera jamais renvoyé et sera donc perdu. A savoir que le message ne requiert donc pas d'être stocké dans le courtier. Ce mode de transfert est le plus rapide des 3.

2. QoS = 1  
**Au moins une fois** spécifie que le message sera envoyé jusqu'à ce qu'un accusé de réception soit reçu par l'émetteur du message. Le message doit donc pouvoir être stocké temporairement par l'émetteur pour pouvoir être renvoyé si jamais. Le message pourra, dans ce cas-ci, être envoyé plusieurs fois.

3. QoS = 2  
**Exactement une fois** est le niveau le plus important, le plus sûr et le plus lent dans MQTT. Il est défini par l'utilisation d'une négociation à deux niveaux entre le client expéditeur et le client abonné. Ce procédé permet de garantir la réception d'une unique copie du message.

<br>


## MQTT & MQ ?

MQTT est bien différent d'un simple protocole de messagerie par file d'attente. Contrairement à ce dernier, en MQTT, le principe est de pouvoir partager un message à tout abonnés s'étant abonné au préalable au sujet et non pas juste à un seul consomateur. Les sujets sont également bien plus flexibles qu'une simple liste d'attente étant donné que ceux-ci peuvent être créés au fur et à mesure.

<br>

## Connexion MQTT via NAT

Généralement un client MQTT, éditeur ou abonné, est placé derrière un routeur qui s'occupera de transformer son ip privée en une ip publique. Le client va tout d'abord initié la connexion en envoyant, au courtier, un message de type CONNECT. Le courtier, possédant une ip publique, maintient la connexion active après le premier CONNECT si aucun problème ne survient. Ceci permet l'envoi et la réception de messages.

## Messages MQTT

### CONNECT (Se connecter)
Si le message d'initiation de connexion CONNECT envoyé au courtier ne respecte pas les spécifications MQTT ou génère un temps trop important entre l'ouverture du socket réseau et l'envoi du message CONNECT, le courtier se charge de mettre fin à la connexion. Ce procéder permet d'éviter toutes tentatives de ralentissement du courtier.

### PUBLISH (Publier)
Dés que le client a établi la connexion avec le courtier, celui-ci peut publier des messages. Comme dit précédemment, le protocole MQTT utilise un filtrage basé sur les sujets des différents messages. Il est impératif que chaque messages contienne une rubrique que le courtier pourra utiliser pour envoyer les messages aux clients abonnés à celles-ci. Dans MQTT, c'est l'éditeur qui choisit le type de structure de donnée a envoyer. Par exemple, celui-ci peut décider d'expédier des données textes, binaires ou encore JSON.

### SUBSCRIBE (S'abonner)
Comme dit plus haut, si aucune personne n'est abonné à un sujet, aucune donnée ne sera jamais transmise. Il est donc indispensable de pouvoir s'abonner. Pour cela, le client voulant s'abonné doit envoyé un SUBSCRIBE au courtier.  

### SUBACK (Accusé d'abonnement)
Pour confirmer à l'abonné qu'il s'est bien abonné à un sujet, le courtier (après avoir bien reçu un message SUBSCRIBE par l'abonné) lui envoie un message d'accusé de réception SUBACK.  
Quand l'envoi du SUBSCRIBE et la réception d'un message SUBACK est réalisé avec succès par l'abonné, celui-ci obtient et obtiendra chaque messages publiés qui correspond aux rubriques liées à ses abonnements qui étaient contenus dans son précédent message SUBSCRIBE.

### UNSUBSRIBE (Se désabonner)
Bien entendu, qui dit abonnement dit désabonnement. Ce message a pour but de supprimer les abonnements qu'à client abonné a chez un courtier.

### UNSUBACK (Accusé de désabonnement)
Pour confirmer le désabonnemnt de l'abonné, le courtier envoie à nouveau un accusé de réception mais UNSUBACK cette fois-ci.

## Conclusion


## Bibliographie :

1) - Lien: https://iot.goffinet.org/iot_protocole_mqtt.html,  
Nom du site: goffinet,  
Nom de l'auteur: /,  
Date de consultation: 25/05/2022,   
Dernière date de modification: /  
<br>

2) - Lien: https://fr.wikipedia.org/wiki/MQTT,  
Nom du site: wikipedia,  
Nom de l'auteur: /,  
Date de consultation: 25/05/2022,  
Dernière date de modification: 28/01/2022  
<br>

3) - Lien: https://fr.wikipedia.org/wiki/Publish-subscribe  
Nom du site: wikipedia,  
Nom de l'auteur: /,  
Date de consultation: 25/05/2022,  
Dernière date de modification: 31/03/2021  
<br>

4) - Lien: https://ably.com/topic/pub-sub  
Nom du site: ably,  
Nom de l'auteur: Matthew O’Riordan,   
Date de consultation: 25/05/2022,    
Dernière date de modification: 17/07/2020  
<br>

5) - Lien: https://stringfixer.com/fr/Publish/subscribe  
Nom du site: stringfixer.com,  
Nom de l'auteur: /,  
Date de consultation: 03/06/2022,  
Dernière date de modification: /  
<br>

6) - Lien: https://www.ibm.com/docs/fr/ibm-mq/7.5?topic=ssfksj-7-5-0-com-ibm-mm-tc-doc-tc60340--htm  
Nom du site: ibm.com,  
Nom de l'auteur: /,  
Date de consultation: 03/06/2022,  
Dernière date de modification: 20/04/2021  
<br>

7) - Lien: https://www.hivemq.com/blog/mqtt-essentials-part-6-mqtt-quality-of-service-levels/  
Nom du site: hivemq.com,  
Nom de l'auteur: The HiveMQ Team,  
Date de consultation: 04/06/2022,  
Dernière date de modification: 16/02/2015  
<br>


8) - Lien: https://www.hivemq.com/blog/mqtt-essentials-part2-publish-subscribe
Nom du site: hivemq.com,  
Nom de l'auteur: The HiveMQ Team,  
Date de consultation: 04/06/2022,  
Dernière date de modification: 19/01/2015  
<br>


8) - Lien: https://www.hivemq.com/blog/mqtt-essentials-part-4-mqtt-publish-subscribe-unsubscribe/
Nom du site: hivemq.com,  
Nom de l'auteur: The HiveMQ Team,  
Date de consultation: 06/06/2015,  
Dernière date de modification: 02/02/2015 
<br>

