# Zabbix

## 1. Introduction
Zabbix est un logiciel de monitoring open source. Projet démarré en tant que logiciel interne en 1998, il est rendu public en 2011 et la première version stable du logiciel, la 1.0, sort en 2004. Actuellement, la dernière version majeure disponible et stable est la 6.0.4 disponible depuis le 3 mai 2022 mais il existe aussi la version 6.2.0 qui est en bêta. Zabbix est utilisé pour la surveillance de divers composant informatique telle que les serveurs, machines virtuelles et services cloud. [1] [7]


## 2. Fonctionnement
L’outil de supervision Zabbix fonctionne avec une partie coté serveur qui s’installe sur un environnement Linux et un coté agent qui peut s’installer sur n’importe quelle plateforme. [2]
Zabbix peut opérer de plusieurs façons pour récolter des informations. Avec un trappeur Zabbix (container dans lequel nos données sont envoyées)[9], c’est l’hôte qui démarre une communication avec le serveur et envoie ses données. Le trappeur permet de mettre en place des scripts afin de définir des variables. Les valeurs contenues dans ces variables sont ensuite transmises au serveur qui lui va traiter ses données et les afficher sur une interface graphique. [2] [3]<br>

Il existe aussi le cas de figure où c’est le serveur qui initie la communication via des requêtes grâce à un agent Zabbix et des paramètres utilisateur. Les paramètres utilisateur vont permettre de définir les éléments à monitorer et les données qui devront être récoltée par l’agent Zabbix. Comme pour le trappeur, les données récoltées vont être récoltées et traitées par le serveur. [2] [4] <br>


Pour assurer la sécurité des communications entre le serveur et les machines clientes, Zabbix va appliquer un système de chiffrement. De cette manière, toutes les données transmissent sont protégées lorsqu’elles circulent sur des réseaux non sécurisés. [5] <br>

Une fois arrivées sur le serveur, les données doivent être stockées. Pour cela, Zabbix utilise une base de données relationnelle sous MySQL ou PostgreSQL. Les données ainsi stockées pourront alors être utilisée par une interface web afin de visualiser le monitoring de nos machines. [5]
Zabbix va permettre ainsi de construire des graphes sur tout un tas de paramètres que l’on décide de monitorer : L’espace disque libre sur un serveur, le taux d’utilisation de la RAM d’une machine, la chaleur du processeur, etc. <br>

![Architecture Zabbix Agent et trappeur](http://www.kjkoster.org/zapcat/Architecture_files/zabbix%20arch.png).

Afin de mieux gérer les comportement non désiré ou état critique d’une infrastructure IT, Zabbix propose un système d’envoi de notification. L’utilisateur installant et paramétrant l’outil de monitoring, va pouvoir définir des seuils pour un élément que l’on veut surveiller. Ainsi lorsque, par exemple, l’espace libre sur le disque de stockage X : de notre serveur sera inférieur à la valeur définie, Zabbix va envoyer une notification par mail, messagerie instantanée ou SMS selon ce qui a été définit. [6] <br>

## 3. Avantages et Inconvénients
### 3.1. Avantages :
* Découverte automatique du réseau[10]
* Intégration SNMP [7]
* Très complet
* Monitoring distribué
* Hautement configurable [8]

Le protocole SNMP permet à une application de gestion (ici Zabbix) de demander des informations provenant d'une unité gérée. L'unité gérée contient un logiciel qui envoie et reçoit des informations SNMP. Ce module logiciel est généralement appelé agent SNMP. [11]

### 3.2. Inconvénients
* Interface très encombrée [8]
* La configuration demande une très grande maitrise de l’outil [8]
* Gourmand en ressources [8]

## 4. Concurrent
Même si Zabbix est un outil de monitoring très populaires à l'heure actuelle, il existe de nombreux autres logiciels de supervision qui ont chacun leurs spécificités. [12]
* Nagios: un de premiers logiciel de ce type ayant vu le jour en 1999 sous le nom de NetSaint.
* DataDog: outil de surveillance prévu plus spécifiquement pour les équipes de développement afin de mettre sous forme de graphes les différentes données récoltées par leurs applications.
* Microsoft System Center: l'outil de surveillance proposé par Microsoft
* Netdata: Conçu et optimisé pour les systèmes Linux
* Chekmk: Extrèmement simple à mettre en place


## 5. Conclusion
Grâce à sa longévité de plus de 20 ans et à chaque ajout et amélioration qu’ont apportés toutes les versions, Zabbix est maintenant une référence connue dans le monde entier en ce qui concerne le monitoring de système informatique. Malgré la forte concurrence dans ce domaine, Zabbix a réussi à innover et à se différencier des autres outils de monitoring existant. <br>


## Sources
* Zabbix. (n.d.). Download million images for free. https://stringfixer.com/fr/Zabbix [1]
* Supervision et métrologie Zabbix : principes et fonctionnement - Syloe. (n.d.). Syloe. https://www.syloe.com/supervision-et-metrologie-zabbix [2]
* Elément trapper. (n.d.). Zabbix :: The Enterprise-Class Open Source Network Monitoring Solution. https://www.zabbix.com/documentation/4.0/fr/manual/config/items/itemtypes/trapper [3]
*  Paramètres utilisateur. (n.d.). Zabbix :: The Enterprise-Class Open Source Network Monitoring Solution https://www.zabbix.com/documentation/4.0/fr/manual/config/items/userparameters  [4]
* Kalsin, V. (2020, July 30). Comment installer et configurer Zabbix et configurer des serveurs à  distance sur Ubuntu 20.04. DigitalOcean  The developer cloud. https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-zabbix-to-securely-monitor-remote-servers-on-ubuntu-20-04-fr [5]
* Logiciel zabbix open source | Glossaire Syloé Experts Devops & Cloud. (n.d.). Syloe. https://www.syloe.com/glossaire/logiciel-zabbix/ [6]
* Notes de publication. (n.d.). Zabbix :: The Enterprise-Class Open Source Network Monitoring Solution. https://www.zabbix.com/fr/release_notes [7]
* Solution ultime de Supervision d'Entreprise - Zabbix. (n.d.). AxelIt. https://www.axelit.fr/technologies/zabbix/ [8]
* Zabbix : Utiliser les Trappers pour faire des Graphs customs - UnGeek.Fr. (n.d.). UnGeek.Fr. https://ungeek.fr/zabbix-trapper-custom-graph/#1-créer-le-trapper [9]
* Configuration d'une règle de découverte réseau. (n.d.). Zabbix :: The Enterprise-Class Open Source Network Monitoring Solution. https://www.zabbix.com/documentation/current/fr/manual/discovery/network_discovery/rule [10]
* IBM Docs. (n.d.). IBM - Deutschland | IBM. https://www.ibm.com/docs/fr/spectrum-control/5.3.7?topic=standards-simple-network-management-protocol [11]
* Alternatives Zabbix. G2 https://www.g2.com/products/zabbix/competitors/alternatives [12]
