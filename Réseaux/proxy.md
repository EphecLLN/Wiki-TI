a[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# Le proxy

## Qu'est-ce qu'un proxy?[^1] [^2]
Tiré de l'anglais qui signifie "procuration", le terme proxy désigne un serveur qui permet de relayer les requêtes.
Un serveur Proxy (également appelé serveur mandataire) est donc un serveur qui agit comme intermédiaire entre un utilisateur demandeur d'une ressource et le serveur distant, fournisseur de cette ressource. L'utilisateur ne peut plus s'adresser directement au serveur distant sans passer par le serveur Proxy, qui fonctionne au nom du client, masquant potentiellement la véritable origine de la demande.
###### Fig.1 Schéma d'un serveur Proxy
<img src="https://www.proxyvpn.fr/wp-content/uploads/2019/02/1200px-CPT-Proxy.svg_.png" width="800" height="250">
###### "https://www.proxyvpn.fr/wp-content/uploads/2019/02/1200px-CPT-Proxy.svg_.png"© proxyvpn.fr


## Que se passe-t-il au niveau du réseau ? [^3]
Le Proxy travaille sur la couche application, au niveau 7 du modèle OSI. Le Proxy décapsule donc toutes les couches inférieures quand il reçoit une requête http et réencapsule les paquets IP pour les retransmettre vers le serveur de destination. Il y a donc deux connexions TCP ce qui signifie une rupture de flux au niveau du Proxy.

###### Fig.2 Décapsulation de la requête reçue et réencapsulation
![Décapsulation de la requête et réencapsulation](https://zestedesavoir.com/media/galleries/5382/2e3db985-cde5-4e61-9bf7-5af14485d872.png)
###### "Décapsulation de la requête et réencapsulation" https://zestedesavoir.com/media/galleries/5382/2e3db985-cde5-4e61-9bf7-5af14485d872.png

Dans un réseau, on peut donc empêcher les utilisateurs, grâce à un Firewall d'accéder directement à Internet et les obliger de passer par un Proxy, ce qui permet de contrôler les accès dans un sens comme dans l'autre.

## Les différentes fonctionnalités d'un serveur Proxy [^2] [^4] [^5]

#### **Amélioration des performances**
* Le caching Proxy ou proxy-cache
Grâce à sa capacité de garder en cache les pages les plus souvent visitées par les utilisateurs du réseau local, le Proxy est capable de réduire le temps d'accès à ces pages ainsi que la bande passante vers Internet.
Le caching proxy a été le premier type de serveur proxy.
* Le PEP (Performance Enhancing Proxy)
C'est un Proxy concu pour améliorer les performances TCP en cas de :
     - temps aller-retour élevés
     - pertes de paquets élevées
     - taux de chargement et téléchargement très différents
Il peut compresser les données afin d'utiliser le réseau plus efficacement ce qui participe également à la rapidité de la navigation.
#### **Anonymisation de l'utilisateur**
Le serveur Proxy transmet la requête au serveur distant via une adresse ip publique et de ce fait masque l'adresse ip initiale, celle de l'utilisateur. La réponse du serveur distant ne se fait que via le Proxy car il est le seul à connaître l'adresse d'origine. L'utilisateur devient invisible ou presque car le trafic reste entièrement visible pour le FAI (Fournisseur d'Accès à Internet) et pour le Proxy lui-même. Un certain degré de confiance doit s'établir entre le serveur Proxy et son utilisateur. 

#### **Journalisation des connexions**
Le Proxy enregistre l'ensemble des requêtes dans les logs et offre ainsi une traçabilité centralisée des flux. Ceci permet, entre autres, de rédéfinir la politique de sécurité du réseau sur base des données récoltées sans oublier, dans le cadre d'une entreprise de concilier RGPD et récolte de ces données parfois à caractère personnel. 

#### **Filtrage et surveillance**
- Contrôle de contenu
     - Un filtrage peut être appliqué suivant la politique de sécurité en place sur le réseau. Ceci permet de bloquer les sites inutiles dans un cadre de travail (pornographie, réseaux sociaux,...) mais également ceux considérés comme malveillants. 
Les requêtes peuvents être filtrées selon :
         - des listes
              - White List -> le filtrage se fait à partir d'une liste de sites autorisés  
              - Black List -> le filtrage se fait à partir d'une liste de sites non autorisés
         - une URL
         - un filtre d'expressions régulières (URL Regex filtering)
         - le MIME
         - des mots-clés du contenu

- Contournement des filtres
     - Dans certains cas, les utilisateurs peuvent contourner les proxies qui filtrent à l'aide de listes noires en envoyant les informations proxy à partir d'un site ne figurant pas sur la liste.
     - Certains serveurs limitent leur service en fonction de l'origine de la requête. Par exemple, en cas de censure gouvernementale qui vise à bloquer l'accès à certains sites dits subversifs en utlisant la géolocalisation basée sur IP. En passant par un proxy situé dans un autre pays, il est possible de contourner le filtrage(5).
     Dans l'autre sens, un internaute étranger pourra accéder à un site qui n'accepte que les internautes d'un pays ciblé en passant par un proxy situé dans le pays en question.

#### **Sécurisation**
Le serveur Proxy protège les utilisateurs de son réseau en servant de visage public unique. Tous ses utilisateurs sont anonymes car ils sont dissimulés par l'adresse IP du Proxy. En cas de tentative de piratage sur une machine spécifique, son adresse IP sera beaucoup plus difficile à trouver. De plus, les Proxies peuvent jouer le rôle de Firewall ce qui augmente encore la sécurité du réseau.

## Types de serveurs Proxy [^1] [^2]
Il existe de nombreux types de serveurs Proxy classés selon leur fonctionnalités décrites ci-dessus. 
#### **HTTP Proxy ou Web Proxy**[^7] [^5]
- Proxy transparent
Ce type de Proxy ne masque pas l'adresse IP de l'utilisateur. Il est utilisé par les écoles, les organisations et les gouvernements afin de pouvoir contrôler l'utilisation d'Internet et ainsi censurer son contenu.
Le serveur Web connaît donc l'adresse IP de l'utilisateur mais il sait également que l'utilisateur utilise un Proxy.
- Proxy anonyme
L'adresse IP de l'utilisateur est remplacée par l'adresse IP du Proxy.
Le serveur Web ne connaît pas l'adresse IP de l'utilisateur mais il sait que l'utiilisateur utilise un Proxy.
- Proxy déformant ou distordant
Le Proxy transmet une fausse adresse IP pour désigner l'utilisateur leurrant ainsi le serveur quant à la source de la requête.
Le serveur Web sait que l'utilisateur utilise un proxy et pense connaître l'adresse IP d'origine.
- Proxy High Anonymity ou Proxy Elite
Il s'agit du type de Proxy le plus sécurisé car il parvient à dissimuler également son existence au serveur en modifiant périodiquement l'adresse IP qu'il présente au serveur Web.
Le serveur Web ne connaît ni l'adresse IP de l'utilisateur ni celle du Proxy.

#### **Proxy SOCK (Socket Secure)** [^6]
Le Proxy SOCK est un Proxy de tunneling, technique consistant à utiliser un protocole pour transporter des données à l'intérieur d'un autre protocole. Il crée une connexion TCP vers un autre serveur derrière le Firewall au nom du client. Le serveur Proxy SOCK relaie la session TCP et UDP d'un utilisateur qui n'est pas autorisé à établir une connexion avec un serveur extérieur au-delà du Firewall.
###### Fig.3 Schéma d'un Proxy de tunneling
![](https://miro.medium.com/max/1378/1*St3AO8UxsllEnO93Pqoqcw.png)
_shivk "https://miro.medium.com/max/1378/1*St3AO8UxsllEnO93Pqoqcw.png"
"Le client SSH crée un proxy SOCK (tunnel). Il prend le trafic local envoyé à un port spécifique du PC et l'envoie via la connexion SSH à un emplacement distant, le serveur SSH"

#### **Proxy FTP (File Transfer Protocol)**[^11]
Celui-ci fonctionne comme un relais pour le transfert de fichiers et permet de contrôler les connexions et les commmandes telles que PUT et GET en fonction des adresses source et de destination et de l'authentification de l'utilisateur. Il peut également réguler la quantité et la fréquence des transferts et limiter la taille des fichiers.

#### **Proxy SIP (Session Initiation Protocol)**[^11]
Basé sur le protocole SIP, le Proxy SIP permet de contrôler les appels au sein d'un réseau. Il redirige les appels selon les informations reprises dans le registre SIP.

#### **Proxy SSL (HTTPS Proxy)**[^11]
Le Proxy SSL effectue le chiffrement et déchiffrement entre l'utilisateur et le serveur de manière totalement invisible.

#### **Proxy SMTP (Simple Mail Transfer Protocol)**[^11]
Le Proxy SMTP contrôle les e-mails, entrants ou sortants, sur base de l'adresse source, le serveur de l'expéditeur ou encore le contenu. Les proxies SMTP sont utilisés pour, entre autres, filtrer les spams, prévenir contre le phishing et autre logiciels malveillants.

#### **Proxy DNS et Proxy Smart DNS**[^11]
Le Proxy DNS transfère les requêtes et les réponses DNS entre l'utilisateur et le serveur. Il peut, entre autres, améliorer les performances grâce à son cache et protèger les serveurs contre le piratage de domaine et l'usurpation de DNS.
Le Proxy Smart DNS se distingue du Proxy DNS par le fait qu'il contourne les restrictions mises en place par les serveurs DNS comme par exemple la géolocalisation.

#### **Reverse Proxy** [^5]
Le Reverse Proxy se situe entre un groupe de serveurs et Internet, il gère le trafic pour ses serveurs.
En bref, le proxy protège les clients, alors qu'un Reverse Proxy protège les serveurs.
Pour plus d'informations sur le Reverse Proxy, se référer à l'[article](https://github.com/KeviinKeurvels/Wiki-TI/blob/reverse_proxy/reverse_proxy.md) de Monsieur Kevin Keurvels.


## Différence entre Proxy et Firewall [^8]
Les Firewalls bloquent les accès non autorisés à des réseaux privés ou depuis ces réseaux privés. Ils empêchent les machines au sein du réseau d'être exposées librement à Internet.
Les principales différences entre un Firewall et un Proxy :
- Un Firewall bloque les connexions, un Proxy les facilite.
- Un Firewall se place entre un réseau privé et public. Un Proxy peut se placer entre deux réseaux publics
- Un Firewall protège un réseau interne contre des attaques tandis qu'un Proxy fournit une anonymisation et contourne des restrictions
- Un serveur Proxy peut faire office de Firewall
Les pare-feu ne peuvent pas être considérés comme des serveurs proxy car même s'ils ont la capacité de bloquer ou d'autoriser les requêtes en fonction de certaines règles, ils ne les acheminent pas.
## Différence entre Proxy et VPN [^9] [^12]
Un VPN (Virtual Private Network) est une connexion sécurisée et chiffrée entre deux réseaux. A l'instar d'un Proxy, il masque l'adresse IP en agissant comme intermédiaire et réoriente le traffic. 
Un VPN ajoute un réseau privé virtuel, une sorte de tunnel personnel qui vous cache aux yeux des autres. Ce tunnel de chiffrement transforme un texte normal en un mélange codé, déchiffrable à l'aide d'une clé de déchiffrement que seuls l'utilisateur et le serveur VPN possèdent.
Les principales différences entre un VPN et un Proxy :
- un VPN fournit un cryptage, une authentification et une protection d'intégrité au trafic tandis que le Proxy ne fournit pas beaucoup de sécurité sur la connexion elle-même.
- un VPN fonctionne sur le Firewall, un Proxy fonctionne sur les navigateurs.
- Un VPN crée un tunnel de connexion, un Proxy ne crée aucun tunnel.
- Les protocoles utilisés sont différents : PPTP, IKEv2, L2TP/IPsec, SSL, TLS...pour un VPN et HTTP, SMTP, FTP... pour un Proxy.
Le VPN et le proxy ont des objectifs similaires, mais un VPN fournit plus de sécurité qu’un serveur proxy.

## Risques liés au Proxy[^4] [^10]
#### Serveur Proxy gratuit
Il y a toujours une contrepartie à la gratuité d'un service. 
- Performances diminuées : quand la charge de trafic est importante, le maintien de la vitesse nécessite une bande passante étendue. Seulement, la plupart des serveurs Proxy gratuits fonctionnent avec une faible bande passante ce qui signifie un ralentissement certain au niveau de la navigation Web. 
- Logiciels espions : ils sont généralement téléchargés comme des applications libres ou transmis par des sites malveillants.
- Sécurité des données : Dans le cas d'un serveur Proxy hacké, les informations personnelles qui transitent via le serveur risquent d'être surveillées, récoltées et modifiées à des fins malveillantes comme l'usurpation d'identité.
#### Historique de navigation
Un Proxy connaît l'IP d'origine et l'historique des requêtes de l'utilisateur. Dès lors, il s'agit de vérifier si le serveur enregistre ces données et quelle politique de conservation des données il applique car si ces données sont consignées et vendues alors que le service attendu est l'anonymisation, le dit-service est frauduleux.
#### Cryptage
Il ne faut pas oublier que de base, un serveur proxy ne chiffre pas le trafic qu’il filtre. Il faut donc associer son utilisation avec un protocole de cryptage sinon les demandes seront envoyées en texte brut et à la merci de celui qui écoute .


## Bibliographie

[^1]: "_Qu'est-ce qu'un proxy?_", proxyvpn.fr, https://www.proxyvpn.fr/qu-est-ce-qu-un-proxy (consulté le 02/05/2022) 
   ** Résumé : Page Web décrivant le Proxy
   ** Avis sur la ressource : Court mais concis pour une première approche 
[^2]: "_Proxy server_", Wikipedia, https://en.wikipedia.org/wiki/Proxy_server (consulté le 02/05/2002)
   ** Résumé : Page wipédia décrivant un serveur Proxy
   ** Avis sur la ressource : Beaucoup plus détaillée que la page en français équivalente 
[^3]: Vince,"_Le proxy dans tous ses états_", zestedesavoir.com, https://zestedesavoir.com/tutoriels/2789/les-reseaux-de-zero/reprenons-du-service/le-proxy-dans-tous-ses-etats/ (consulté le 06/05/2022)
   ** Résumé : Le fonctionnement du proxy plus détaillé
   ** Avis sur la ressource : Un des seuls sites où l'on aborde la couche OSI
[^4]: Jeff Petter, "_What is a Proxy server and how does it work?_", varonis.com, https://www.varonis.com/blog/what-is-a-proxy-server 27/03/2020(consulté le 06/05/2022)
   ** Résumé : Le fonctionnement du Proxy, risques et avantages
   ** Avis sur la ressource : Très claire mais un peu simple dans la description
[^5]: "_Qu'est-ce qu'un serveur proxy et comment fonctionne-t-il ?_", avast.com, https://www.avast.com/fr-fr/c-what-is-a-proxy-server (consulté le 04/05/2022)
   ** Résumé : Un récap du Proxy
   ** Avis sur la ressource : La page est claire mais Avast en profite pour faire sa publicité
[^6]: _shivk, "_Adventure with Proxy : Intro to Proxy_", medium.com, https://medium.com/@_shivk/adventure-with-proxy-intro-to-proxy-f7e640b455e7 février 2020 (consulté le 06/05/2022)
   ** Résumé : Le Proxy et ses différents types
   ** Avis sur la ressource : Très claire au niveau des différents Proxies
[^7]: "_What is an HTTP Proxy? Types of HTTP Proxies explained_", bestproxyreviews.com, https://www.bestproxyreviews.com/http-proxy/ 21/04/2022(consulté le 06/05/2022)
   ** Résumé : Différents types de Proxies et leurs fournisseurs
   ** Avis sur la ressource : Très claire au niveau des différents Proxies
[^8]: "_Différence entre Proxy et Firewall_", waytolearnx.com, https://waytolearnx.com/2018/09/difference-entre-proxy-et-firewall.html 09/09/2018(consulté le 02/05/2022)
   ** Résumé : Un rappel sur le Proxy et sur le Firewall
   ** Avis sur la ressource : Juste ce qu'il faut sans entrer dans les détails
[^9]: "_Différence entre VPN et Proxy_, waytolearnx.com, "https://waytolearnx.com/2018/07/difference-entre-vpn-et-proxy.html 28/07/2018(consulté le 02/05/2022)
   ** Résumé : Un rappel sur le Proxy et sur le VPN
   ** Avis sur la ressource : Juste ce qu'il faut sans entrer dans les détails
[^10]: Vuk Mujovic, "_Cacher les connexions Proxy: qu'est-ce qu'un Proxy et une adresse IP?_", le-vpn.com, https://www.le-vpn.com/fr/quest-ce-quun-proxy/#:~:text=Risques%20li%C3%A9s%20%C3%A0%20l'utilisation%20d'un%20proxy&text=Le%20risque%20le%20plus%20courant,transmis%20par%20des%20sites%20malveillants. 29/10/2021(consulté le 08/05/2022)
   ** Résumé : Les fonctionnalités du Proxy, les types et les risques liés au Proxy
   ** Avis sur la ressource : Concis avec en prime une liste des serveurs Proxy les plus connus
[^11]: Catherine Chipeta, "_Qu'est-ce qu'un serveur Proxy? Une explication claire de son fonctionnement_", upguard.com, https://www.upguard.com/blog/proxy-server#:~:text=Proxy%20servers%20work%20by%20facilitating,directly%20back%20to%20the%20user. 18/04/2022(consulté le 07/05/2022)
   ** Résumé : Le fonctionnement et les types de Proxies
   ** Avis sur la ressource : Aussi claire que le titre
[^12]: "_Qu'est-ce qu'un VPN et comment fonctionne-t-il ?_", avast.com, https://www.avast.com/fr-fr/c-what-is-a-vpn (consulté le 04/05/2022)
   ** Résumé : Un récap du VPN
   ** Avis sur la ressource : La page est claire mais Avast en profite pour placer sa publicité