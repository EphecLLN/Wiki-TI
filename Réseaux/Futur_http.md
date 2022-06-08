[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# L’avenir de HTTP

## Définition [^1]**

L’Hypertext Transfer Protocol, généralement abrégé HTTP, littéralement « protocole de transfert hypertexte », est un protocole de communication client-serveur développé pour le web.
Le protocole HTTP est devenu un standard de l’IETF en 1997.

## Evolution des versions du protocole HTTP [^2]

Plusieurs versions de ce protocole se sont succédées depuis l’origine : HTTP/0.9, HTTP/1.0, HTTP/1.1 et HTTP/2.
Les versions successives du protocole vont progressivement corriger les problèmes des versions précédentes

## HTTP/2 [^3]**

La version HTTP/2 est toujours la version majoritaire actuellement.
Par rapport à ses prédécesseurs, elle a introduit le concept de  multiplexage de plusieurs transactions HTTP dans une seule connexion TCP.
Ce concept visait à résoudre le problème du blocage de Head Of Line (HOL), qui est l’un des principaux problèmes de performance rencontrés.
Ce multiplexage est malheureusement inefficace s’il existe des pertes de paquets. 

De plus, les pages web reprennent des contenus de plus en plus lourds (vidéos 4K, images, rendus 3D etc.), et le trafic ne cesse d’augmenter.

C’est dans ce contexte que HTTP/3 a été démarré pour aider à améliorer les performances du web.


## Vers l’avenir de HTTP [^4]**
Sans disposer de boule de cristal, on peut raisonnablement penser que l’avenir de HTTP va se baser sur plusieurs normes qui correspondent à des avancées 
majeures depuis la première version de HTTP.
Il s’agit des normes suivantes :

-	HTTPS : [RFC 2818]
-	HSTS : [RFC 6797]
-	QUIC : [RFC 9000] est la norme principale, décrivant le socle de base de QUIC
-	HTTP/3 [Toujours En cours de standardisation par l’IETF : dernier draft date du 11 mars 2022 :  Hypertext Transfer Protocol Version 3 (HTTP/3)]
-	HTTPA : hors norme actuellement

Ces différentes normes seront détaillées ci-dessus 

## HTTP/3 [^3] [^5] [^6] [^7] [^8] [^9]**

Un certain nombre de différences importantes existent entre HTTP/2 et HTTP/3.

![image](https://user-images.githubusercontent.com/56791491/172709717-23d03c0a-e536-4685-b266-cf3d1f0bf79d.png)

### TCP vs UDP
Contrairement à HTTP/2, HTTP/3 n'utilise pas le protocole de contrôle de transmission (TCP) comme le faisaient ses prédécesseurs.
Au lieu de cela, il utilise le protocole de connexion Internet rapide UDP.

Actuellement, le HTTP/2 embarque en effet un protocole de transport TCP orienté “connexion”.
En résumé, lorsqu'une machine A envoie des données à une machine B, cette dernière est prévenue de l'arrivée des données et envoie un accusé de réception.
Ce protocole présente un avantage : il renforce la sécurité. En revanche, il rend le transport des données relativement lent, puisqu'il nécessite plusieurs allers-retours.

![image](https://user-images.githubusercontent.com/56791491/172709855-94af9cca-4e25-457a-9751-ec33d0987ce9.png)

Le protocole QUIC utilise UDP, qui est un protocole de transport orienté “non connexion”.
Lorsqu'une machine A envoie des paquets à une machine B, le flux est unidirectionnel.
La machine B n'est pas prévenue de l'arrivée des données et les reçoit sans envoyer d'accusé de réception.

QUIC garantit la livraison des flux dans l'ordre, mais pas entre les flux.
Cela signifie que chaque flux enverra des données et maintiendra l’ordre des données,
mais chaque flux pourra atteindre la destination dans un ordre différent de celui que l’application a envoyé.

### TLS 1.3

Dans les faits, ce protocole UDP n’est pas un transport fiable. Pour éviter ce problème, QUIC ajoute une couche au-dessus d'UDP qui introduit la fiabilité.
L’objectif est de garantir la même fiabilité des communications, contrôle de flux et gestion de la congestion similaires à TCP.

Ainsi QUIC peut envoyer d’un seul coup à un serveur les requêtes de connexion et de chiffrement, ce qui réduit de moitié la latence des nouvelles connexions. 

En résumé, QUIC accélère la navigation web tout en améliorant la sécurité. HTTP/3 ne prend en charge que les connexions cryptées grâce au cryptage TSL 1.3 intégré.


### Avantages de l’HTTP/3 

Les avantages de HTTP/3 sont une meilleure vitesse de transmission, des temps de chargement plus courts et une connexion plus stable.
S'appuyant sur UDP, HTTP/3 contourne les points faibles de TCP et utilise tous les avantages de HTTP/2 et HTTP sur QUIC.

#### Résolution du problème de la latence due au TCP

Lorsqu’une session TCP est établie, elle déclenche généralement l’algorithme de contrôle de congestion TCP.
Il évalue la quantité de trafic qui peut être envoyée avant que la congestion ne se produise par le biais du processus de démarrage lent TCP (Slow Start).

En s’appuyant sur le protocole UDP et en prenant en charge les flux de première classe, QUIC résout ce problème de latence lié au démarrage lent de la connexion.


#### Le problème de blocage de HOL est résolu

QUIC offre un support au multiplexage, de sorte que différents flux HTTP peuvent utiliser différents flux de transport QUIC tout en partageant la même connexion QUIC.
Ainsi, l’état de congestion est partagé.
Les flux QUIC sont transmis indépendamment et, dans la plupart des cas, la perte de paquets affectant un flux n’affecte pas les autres.

### Quels problèmes HTTP/3 pourrait-il poser ? [^7]

De nombreux critiques indiquent que HTTP/3 arrive trop tôt après le protocole HTTP/2. Les problèmes suivants sont souvent cités.

#### Ossification

L’Internet est rempli d’équipements divers comme des routeurs ou pare-feu, qui exécutent généralement des logiciels pour gérer le trafic réseau et qui sont rarement mis à jour.  

Ce phénomène est souvent appelé « ossification ». Cela signifie que beaucoup de ces éléments ne sont pas encore adaptés au protocole QUIC.

#### Problèmes de sécurité

La sécurité des applications et des données est au centre des critiques.
Grâce à une réglementation claire des requêtes et des réponses, TCP était considéré comme un protocole fiable et orienté connexion. 

Or, puisque les contrôles de sécurité et le cryptage n'ont plus lieu via TLS, mais directement via UDP, les fournisseurs ont peur d’un impact sur la sécurisation du trafic de données en raison du manque d'authentification TLS. En conséquence, certains réseaux bloquent sciemment QUIC !


#### Performances 

Bien que les performances soient théoriquement améliorées par le passage à HTTP/3, certains maillons risquent de poser problème dans la chaîne et devront être adaptés pour procurer les avancées espérées. 

Ainsi, pour l’optimisation des performances, beaucoup d’appareils prennent en charge la fonction de déchargement TCP (“TCP offloading”) au niveau de leurs cartes d’interface réseau, mais peu d’entre eux le prennent en charge pour UDP.

Par rapport à son homologue TCP, UDP souffre également d’un manque de support des API. C’est également le cas pour les bibliothèques TLS sur QUIC. 

TCP est le protocole courant depuis des années, alors que UDP ne l’est pas, de sorte que les systèmes d’exploitation et la pile logicielle ne sont généralement pas aussi optimisées.
Par conséquent, il y a beaucoup plus de charge/besoins CPU avec QUIC : selon certaines estimations, deux fois plus qu’avec HTTP/2.


### Usage de HTTP/3 [^10] [^11]**

Les motivations pour introduire le nouveau protocole sont donc nombreux :
-	Amélioration de performances 
-	Meilleur temps de réponse
-	Meilleur comportement lors de pertes de paquets
-	Amélioration de la confidentialité
Bien que HTTP/3 soit déjà disponible actuellement, son usage reste minoritaire, comme on peut le consulter sur le site https://w3techs.com/technologies/comparison/ce-http2,ce-http3

![image](https://user-images.githubusercontent.com/56791491/172710600-7cd1777e-57bb-49cf-83cc-314544a62687.png)

Pourtant, de nombreux browsers permettent maintenant d’utiliser HTTP/3 ( voir la liste via l’URL suivante https://caniuse.com/http3) : 

![image](https://user-images.githubusercontent.com/56791491/172710586-cd925436-1f5b-4f18-b59f-a8dcd13e2418.png)

Et de nombreux sites l’utilisent déjà comme google (évidement) mais aussi Youtube ou Facebook
 
![image](https://user-images.githubusercontent.com/56791491/172710551-471dea08-50e9-45d5-908a-35e68e5f94ea.png)



## HTTPS  [^12]**

L'HyperText Transfer Protocol Secure HTTPS signifie  « protocole de transfert hypertextuel sécurisé ») est la combinaison du HTTP avec une couche de chiffrement comme SSL ou TLS.

HTTPS permet au visiteur de vérifier l'identité du site web auquel il accède, grâce à un certificat d'authentification émis par une autorité tierce, réputée fiable.
Il garantit théoriquement la confidentialité et l'intégrité des données envoyées par l'utilisateur (notamment des informations entrées dans les formulaires) et reçues du serveur.
Il peut permettre de valider l'identité du visiteur, si celui-ci utilise également un certificat d'authentification client.

Les parties prenantes sont 
-	le client — par exemple le navigateur Web — contacte un serveur — par exemple Wikipédia — et demande une connexion sécurisée, en lui présentant un certain nombre de méthodes de chiffrement de la connexion (des suites cryptographiques) ;
-	le serveur répond en confirmant pouvoir dialoguer de manière sécurisée et en choisissant dans cette liste une méthode de chiffrement et surtout, en produisant un certificat garantissant qu'il est bien le serveur en question et pas un serveur pirate déguisé (on parle de l'homme du milieu).
-	Ces certificats électroniques sont délivrés par une autorité tierce (autorité de certification) à laquelle le client comme le serveur ont choisi de faire confiance, un peu comme un notaire dans la vie courante, et le client peut vérifier, grâce à la signature de cette autorité sur le certificat présenté par le serveur, que celui-ci est authentique. Le client s’assure par ailleurs que le certificat n’est pas périmé et aussi éventuellement que l’autorité de certification ne l’a pas révoqué.

Le certificat contient aussi une clé dite publique qui permet de chiffrer un message pour le rendre secret et uniquement déchiffrable par le serveur grâce à une clé dite privée que seul le serveur détient, on parle de chiffrement asymétrique.
Cela permet au client d'envoyer de manière secrète un code aléatoire qu’il invente (une clé symétrique dite clef de session) qui sera mélangé à tous les échanges entre le serveur et le client de façon à ce que tous les contenus de la communication soient chiffrés. Pour cela, on mélange le contenu avec le code, ce qui donne un message indéchiffrable, et à l'arrivée refaire l’opération symétrique avec ce message redonne le contenu en clair.

En bref : serveur et client se sont reconnus, ont choisi une manière de chiffrer la communication et se sont passés de manière chiffrée un code (clé de chiffrement symétrique) pour dialoguer de manière secrète.

HTTP/3 n'existe pas dans une version non sécurisée ou non chiffrée.


## HSTS [^13] [^14]**

Une autre avancée pour sécuriser un site web est d’utiliser le HSTS « HTTP Strict Transport Security ». 

HTTP Strict Transport Security (HSTS) est un mécanisme de politique de sécurité proposé pour HTTP, permettant à un serveur web de déclarer à un navigateur web qu'il doit interagir avec lui en utilisant une connexion sécurisée (comme HTTPS).
Elle est communiquée au client via la réponse HTTP, et plus précisément dans le champ d'en-tête nommé « Strict-Transport-Security ». Elle spécifie une période de temps durant laquelle l'utilisateur doit accéder au Serveur informatique uniquement de façon sécurisée.

Le HSTS force les navigateurs et les applications à utiliser — si cela est possible — le HTTPS pour se connecter. 

Facebook, Google, Gmail, Twitter et PayPal ne sont que quelques-uns des grands réseaux sociaux et portails de paiement à déployer une politique de sécurité HSTS aujourd’hui.


## HTTPA [^15] [^16] [^17]**

Cette nouvelle avancée est beaucoup moins aboutie. Elle permet d’ajouter à HTTPS qui sécurise le transport des données un protocole pour garantir également leur protection lors du traitement en protégeant le calcul en cours d'exécution.

« HTTPS est actuellement le principal protocole pour les applications Web. Il fournit une connexion rapide et sécurisée avec un certain niveau de confidentialité et d'intégrité.
Cependant, HTTPS ne peut pas fournir de garanties de sécurité sur les données d'application dans le calcul, de sorte que l'environnement informatique présente des risques et des vulnérabilités.

Compte tenu de cela, Gordon King et Hans Wang, deux employés d'Intel pensent que les services Web peuvent être rendus plus sûrs non seulement en effectuant des calculs dans des environnements d'exécution à distance de confiance, ou TEE, mais également en vérifiant pour les clients que cela a été fait.

Dans un article intitulé : « HTTPA : HTTPS Attestable Protocol », récemment publié sur ArXiv, ils décrivent un protocole HTTP appelé HTTPS Attestable (HTTPA) pour améliorer la sécurité en ligne grâce à la certification à distance.

HTTPA est conçu pour fournir une certification à distance et des garanties informatiques confidentielles entre un client et un serveur lors de l'utilisation du Web sur Internet.
Dans le cas de HTTPA, nous supposons que le client est digne de confiance et que le serveur ne l'est pas. L'utilisateur client peut vérifier ces garanties pour décider s'il peut faire confiance et exécuter les traitements informatiques sur le serveur ou non. Cependant, HTTPA n'offre aucune garantie que le serveur est digne de confiance. Elle comporte deux parties : la sécurisation des communications et des traitements

-	Concernant la sécurité des communications, HTTPA reprend toutes les hypothèses de HTTPS pour la sécurité des communications, y compris l'utilisation de TLS et la communication sécurisée, notamment l'utilisation de TLS et la vérification de l'identité de la personne. 
-	En ce qui concerne la sécurité informatique, le protocole HTTPA nécessite de fournir un état d'assurance supplémentaire d'attestation à distance pour que les  traitements informatiques se produisent dans un espace sécurisé, afin que l'utilisateur client puisse exécuter les calculs  dans une mémoire chiffrée.

Quant à savoir si ou quand HTTPA pourrait être adopté, il n'est pas clair.
Lorsqu'on leur a demandé s'il était prévu de soumettre la spécification sous forme de RFC ou d'entreprendre une autre forme de normalisation,
ils ont répondu : « Nous avons des discussions en cours qui doivent être examinées par l'équipe juridique d'Intel avant de pouvoir adopter HTTPA ».


## Bibliographie

[^1]: "Hypertext Transfer Protocol", fr.wikipedia.org,  https://fr.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP/3  (consulté le 01/06/2022) 
   ** Avis de la source : fiable pour la définition  
   
[^2]: "HTTP/3" : le passé, le présent et l'avenir, https://blog.cloudflare.com/fr-fr/http3-the-past-present-and-future-fr-fr/ (consulté le 01/06/2022)
   ** Avis de la source : document ancien mais qui explique la l’évolution du protocole
   
[^3]: "Qu’est-ce que HTTP/3 – Informations sur le nouveau protocole UDP rapide ?", kinsta.com,
https://kinsta.com/fr/blog/http3/#http3-coming, (consulté le 04/06/2022)
   ** Avis de la source : résumé  de la différence UDP-TCP
   
[^4]: "HTTP/3 : Des origines à nos jours", blog.cloudflare.com, https://blog.cloudflare.com/fr-fr/http-3-from-root-to-tip-fr-fr/ (consulté le 01/06/2022) 
   ** Avis de la source : présentation de l’importance des normes
   
[^5]: "HTTP/3", en.wikipedia.org, https://en.wikipedia.org/wiki/HTTP/3 (consulté le 01/06/2022) 
   ** Avis de la source : Bon résumé
   
[^6]: "HTTP/3 : le protocole origine Google est dans les starting-blocks", silicon.fr, https://www.silicon.fr/http3-protocole-262163.html, (consulté le 02/05/2022) 
   ** Avis de la source : comparaison HTTP/2 vs HTTP3

[^7]: "HTTP/3: the next Hypertext Transfer Protocol explained simply", ionos.com ,https://www.ionos.com/digitalguide/hosting/technical-matters/http3-explained/ (consulté le 04/06/2022) 
   ** Avis de la source : document qui date de 2 ans mais qui résume bien HTTP/3

[^8]: "HTTP/3", http3-explained.haxx.se, https://http3-explained.haxx.se/fr/h3, (consulté le 31/05/2022) 
   ** Avis de la source : très complet et très clair source principale de la partie HTTP/3
[^9]: "Qu’est-ce que le protocole HTTP/3, et ce qu’il change par rapport à HTTP/1 et HTTP/2", fasterize.com, https://www.fasterize.com/fr/blog/protocole-http3/ (consulté le 31/05/2022) 
   ** Avis de la source : information sur QUIC
   
[^10]: Can I use , caniuse.com, https://caniuse.com/http3, (consulté le 02/06/2022) 
   ** Avis de la source : très bonne source d’information pour les compatibilités browser protocole
[^11]: Usage statistics of HTTP/3 for websites, w3techs.com,  https://w3techs.com/technologies/details/ce-http3 (consulté le 02/06/2022) 
   ** Avis de la source : très bonne source d’information actualisé
[^12]: "HyperText Transfer Protocol Secure", fr.wikipedia.org,  https://fr.wikipedia.org/wiki/HyperText_Transfer_Protocol_Secure, (consulté le 1/06/2022) 
   ** Avis de la source : très complet et source la plus importante pour la partie HTTPS
[^13]: "Qu’est-ce que le HSTS et comment le met-on en œuvre ?", globalsign.com, https://www.globalsign.com/fr/blog/qu-est-ce-que-le-hsts-comment-le-mettre-en-uvre
[^14]: "HTTP Strict Transport Security", wikipedia.org , https://fr.wikipedia.org/wiki/HTTP_Strict_Transport_Security, (consulté le 2/06/2022) 
   ** Avis de la source : résumé vulgarisé
   
[^15]: "HTTPA : vers une attestation d’intégrité sur TLS ?", silicon.fr,https://www.silicon.fr/httpa-attestation-integrite-tls-418933.html, (consulté le 2/06/2022) 
   ** Avis de la source : description technique de HTTPA
   
[^16]: HTTPA, a protocol for web services in trusted environments, blog.desdelinux.net/, https://blog.desdelinux.net/en/Http-a-protocol-for-web-services-in-trusted-environments/ (consulté le 2/06/2022) 
   ** Avis de la source : bonne entrée en matière
   
[^17]: "Vous avez entendu parler de HTTPS. Découvrez maintenant HTTPA : des services Web dans des environnements de confiance", securite.developpez.com, https://securite.developpez.com/actu/328186/Vous-avez-entendu-parler-de-HTTPS-Decouvrez-maintenant-HTTPA-des-services-Web-dans-des-environnements-de-confiance-avec-Intel-SGX-un-outil-qui-fournit-le-chiffrement-en-memoire/ (consulté le 2/06/2022) 
   ** Avis de la source : très bonne source 

   
