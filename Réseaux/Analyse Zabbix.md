# Zabbix

## 1. Introduction
Zabbix est un logiciel de monitoring open source. Projet démarré en tant que logiciel interne en 1998, il est rendu public en 2011 et la première version stable du logiciel, la 1.0, sort en 2004. Actuellement, la dernière version majeure disponible et stable est la 6.0.4 disponible depuis le 3 mai 2022 mais il existe aussi la version 6.2.0 qui est en bêta. Zabbix est utilisé pour la surveillance de divers composant informatique telle que les serveurs, machines virtuelles et services cloud. [1] [7]


## 2. Fonctionnement
L’outil de supervision Zabbix fonctionne avec une partie coté serveur qui s’installe sur un environnement Linux et un coté agent qui peut s’installer sur n’importe quelle plateforme. [2]
Zabbix peut opérer de plusieurs façons pour récolter des informations. Avec un trappeur Zabbix, c’est l’hôte qui démarre une communication avec le serveur et envoie ses données. Le trappeur permet de mettre en place des scripts afin de définir des variables. Les valeurs contenues dans ces variables sont ensuite transmises au serveur qui lui va traiter ses données et les afficher sur une interface graphique. [2] [3]<br>

Il existe aussi le cas de figure où c’est le serveur qui initie la communication via des requêtes grâce à un agent Zabbix et des paramètres utilisateur. Les paramètres utilisateur vont permettre de définir les éléments à monitorer et les données qui devront être récoltée par l’agent Zabbix. Comme pour le trappeur, les données récoltées vont être récoltées et traitées par le serveur. [2] [4] <br>

Pour assurer la sécurité des communications entre le serveur et les machines clientes, Zabbix va appliquer un système de cryptage. De cette manière, toutes les données transmissent sont protégées lorsqu’elles circulent sur des réseaux non sécurisés. [5] <br>

<Une fois arrivées sur le serveur, les données doivent être stockées. Pour cela, Zabbix utilise une base de données relationnelle sous MySQL ou PostgreSQL. Les données ainsi stockées pourront alors être utilisée par une interface web afin de visualiser le monitoring de nos machines. [5]
Zabbix va permettre ainsi de construire des graphes sur tout un tas de paramètres que l’on décide de monitorer : L’espace disque libre sur un serveur, le taux d’utilisation de la RAM d’une machine, la chaleur du processeur, etc. <br>

Afin de mieux gérer les comportement non désiré ou état critique d’une infrastructure IT, Zabbix propose un système d’envoi de notification. L’utilisateur installant et paramétrant l’outil de monitoring, va pouvoir définir des seuils pour un élément que l’on veut surveiller. Ainsi lorsque, par exemple, l’espace libre sur le disque de stockage X : de notre serveur sera inférieur à la valeur définie, Zabbix va envoyer une notification par mail, messagerie instantanée ou SMS selon ce qui a été définit. [6] <br>

## 3. Avantages et Inconvénients
### 3.1. Avantages :
* Découverte automatique [2]
* Intégration SNMP [7]
* Très complet
* Monitoring distribué
* Hautement configurable [8]

### 3.2. Inconvénients
* Interface très encombrée [8]
* La configuration demande une très grande maitrise de l’outil [8]
* Gourmand en ressources [8]

## 4. Conclusion
Grâce à sa longévité de plus de 20 ans et à chaque ajout et amélioration qu’ont apportés toutes les versions, Zabbix est maintenant une référence connue dans le monde entier en ce qui concerne le monitoring de système informatique. Malgré la forte concurrence dans ce domaine, Zabbix a réussi à innover et à se différencier des autres outils de monitoring existant. <br>

J’avais personnellement choisi ce sujet car je suis amené à utiliser quotidiennement l’outil Zabbix dans le travail étudiant que j’occupe en attendant de terminer mes études. Mais malgré mon utilisation quotidienne de cet outil afin de monitorer les infrastructures informatiques de client, je ne savais rien du fonctionnement et de l’implémentation de cet outil. Cet article m’a permis de mieux comprendre un outil que de nombreux administrateurs systèmes et réseaux utilisent quotidiennement. <br>

## Sources
* https://stringfixer.com/fr/Zabbix [1]
* https://www.syloe.com/supervision-et-metrologie-zabbix [2]
* https://www.zabbix.com/documentation/4.0/fr/manual/config/items/itemtypes/trapper [3]
* https://www.zabbix.com/documentation/4.0/fr/manual/config/items/userparameters [4]
* https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-zabbix-to-securely-monitor-remote-servers-on-ubuntu-20-04-fr [5]
* https://www.syloe.com/glossaire/logiciel-zabbix/ [6]
* https://www.zabbix.com/fr/release_notes [7]
* https://www.axelit.fr/technologies/zabbix/ [8]