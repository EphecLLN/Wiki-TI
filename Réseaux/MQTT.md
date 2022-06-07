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

- Le domaine de donnée est appelé le **broker**.
- Le premier type d'acteur est le **diffuseur** ou plus communément qualifié le **publisher**.  
*-> Les acteurs publiant des données sur le broker sont qualifiés de diffuseurs.*
- Le second type est le **destinataire** ou **subscriber**.  
*-> Les acteurs récupèrant les informations publiées sur le broker sont qualifiés de destinataires.*

Un diffuseur n'envoie des messages/données pour, à priori, aucun distanateur bien spécifique. A la place, les messages sont envoyé à une catégorie bien spécifique présente sur un broker. *Il peut, bien entendu, il y avoir une multitude de catégories différentes présentes sur le broker.* 

Mais comment recevoir les données dans ces circonstances vous demandez-vous ? Et bien tout simplement par principe d'abonnement. Un destinataire souhaitant recevoir des données s'abonnera à la ou les catégories souhaitées et recevra donc les données présentes dans celles-ci.

Bien entendu, un diffuseur ne peut donc pas savoir si ses messages envoyés au brokers seront lus ou non par un destinataire quelconque. Idem pour les destinataires qui pourront s'abonner aux catégories souhaitées mais n'aurons pas connaissance de l'existance d'un diffuseur lié à cette catégorie. Seul le broker est connu des différents diffuseurs et  destinataires.

![](https://github.com/M-Momin/Wiki-TI/blob/main/Assets/Images/MQTT_publish-subscribe_sh%C3%A9ma.png)

*Shéma réalisé via draw.io*

### Dissociation ? [1]

Un des élément fondamentale spécifique à ce modèle est la dissociation complète entre les diffuseurs et les destinataires et ce sur plusieurs plans.  

- La **dissociation spatiale**: Les acteurs n'ont pas besoin de connaitre les informations de l'autre tel que l'adresse IP ou encore le port.
- La **dissociation temporelle**: Le diffuseur ainsi que le destinataire ne doivent pas nécessairement fonctionner au même instant pour communiquer. 
- La **dissociation de synchronisation**: Se traduit par le fait que l'envoie ou la réception d'un élèment n'interrompt pas les opérations s'exécutant sur les deux acteurs.

### Le filtrage [1][5]

Il est également interressant de savoir, qu'avec l'aide d'un filtrage bien défini, les messages peuvent être reçus seulement par certains destanataires bien spécifiques.

Il existe 4 types de filtrage différents dans le cas du modèle "pub-sub":

1. **Filtrage par sujet**:
Les messages envoyés sont reçus et stockés dans des catégories logiques plus communément appelés "sujets". Les différents destinataires d'un système, qui sont abonnés aux différents sujets visés, auront accès à toutes les informations envoyés et stockés dans ces catégories logiques spécifiques.

2. **Filtrage basé sur le contenu**:
Ici, les messages sont filtrés selon leurs attributs et/ou leur contenu. Un abonné recevra donc uniquement les messages correspondant aux différentes contraintes qu'il a spécifié. Il est important de préciser que cette méthode possède un inconvénient qui est que le contenu doit être connu avant toutes choses par l'abonné ainsi que par l'émetteur pour fonctionner correctement. 

3. **Filtrage par type**:
Dans ce cas-ci, le filtrage se fait en fonction de l'évènement ou du type. Il est coutume d'utiliser cette méthode dans les languages qualifiés d'orientés objet.

4. **Filtrage hybride**
Dans certaines situations, la combinaison du filtrage par sujet et de celui basé sur le contenu est d'application. Dans un premier temps, des messages sont publiés dans des sujets spécifiques (Filtrage par sujet). Par la suite, les abonnés récupèrent des messages basés sur le contenu de différents sujets correspondant à leurs spécifications ultérieures (Filtrage basé sur le contenu).

### Points importants à savoir [1]

Dans le modèle pub/sub, il est important de connaitre, au préalable, les différents détails et aspects qui définissent la structuration des données établies (Différents contenus, attributs, sujets ...). Comme nous avons pu le constater dans le cas du filtrage basé sur le contenu, les 2 acteurs doivent connaitre les différents sujets avant quoi que ce soit.
Ensuite, il faut bien comprendre que les messages envoyés par un diffuseur ne seront peut-être jamais reçus/lus par un destinataire.


## MQTT dans tout ça ?

### MQTT - Modèle pub/sub [1][2]

MQTT découpe l'espace en 2 parties distinctes composées de l'émetteur (publisher) et du destinataire (subscriber). Ceux-ci doivent uniquement avoir pris connaissance de l'adresse ip et du port du broker afin de pouvoir, dans le cas de l'émetteur, envoyer des messages et, dans le cas du destinataire, s'abonner afin d'avoir la possibilité de recevoir des messages spécifiques. MQTT se dissocie donc d'une certaine manière spatialement.

Généralement, l'utilisation de MQTT concerne une conversation quasi instantannée entre le diffuseur et le destinataire. Quand ce n'est pas le cas, une solution de repli existe car MQTT se dissocie temporellement. Le broker peut donc stockés les informations afin qu'elles soient accessibles plus tard pour les distanataires qui ne sont pas actuellement connectés. Pour que cela se fasse, il faut respecter deux conditions:
1. Le destinataire s'est déja connecté une fois avec une session persistante.
2. Le destinataire possède sur la catégorie spécifique une QoS¹ de 1 ou 2.

MQTT peut également faire de la dissocation de synchronisation. Une bonne partie des bibliothèques clientes fonctionnent, à ce jour, de manière asynchrone à l'aide de fonction de rappels. Cela signifie que MQTT n'empêchera aucune tâches s'exécutant pendant la publication ou l'attente d'un message.

MQTT fonctionne avec un filtrage par sujet. Chaque message émis par un diffuseur au broker contient donc un sujet. Le broker utilise ce sujet afin de vérifier si oui ou non un destinataire peut recevoir ce message spécifique.

*QoS¹: la qualité de service décrit la capacité de transmettre des informations dans de bonnes et certaines conditions (les QoS concernant MQTT seront vues plus tard dans l'article.* 

### MQTT - Qualités de services [6][7]

Il existe trois types différents de qualités de services pour le protocole MQTT. A savoir que chaque connexion au broker est spécifiée par une valeur allant de 0 à 2. 

1. QoS = 0  
**Au plus une fois** qui signifie que le message sera envoyé uniquement envoyé une fois au destinataire sans accusé de réception. Si le destinataire n'est pas en ligne ou que le message se perd, il ne sera jamais renvoyé et sera donc perdu. A savoir que le message ne requiert donc pas d'être stocké dans le broker. Ce mode de transfert est le plus rapide des 3.

2. QoS = 1  
**Au moins une fois** spécifie que le message sera envoyé jusqu'à ce qu'un accusé de réception soit reçu par l'émetteur du message. Le message doit donc pouvoir être stocké temporairement par l'émetteur pour pouvoir être renvoyé si jamais. Le message pourra, dans ce cas-ci, être envoyé plusieurs fois.

3. QoS = 2
**Exactement une fois** est le niveau le plus important, le plus sûr et le plus lent dans MQTT. Il est défini par l'utilisation d'une négociation à deux niveaux entre le client expéditeur et le client destinataire. Ce procédé permet de garantir la réception d'une unique copie du message.

<br>


## Avantages et Inconvénients ?


<br>

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
