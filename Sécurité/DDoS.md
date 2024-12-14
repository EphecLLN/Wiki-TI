# Attaques par déni de service distribué (DDoS) et protection 

## C'est quoi une attaque DDoS
### Qu'est ce que c'est ?
Une attaque DDoS c'est une cyberattaque qui a pour objectf de rendre un site web ou un service indisponible en envoyant énormément de trafic sur celui-ci. Cela surcharge les ressources du système et le rend inaccessible aux utilisateurs.

Le but d'une attaque DDoS est donc de perturber les services et de rendre un site ou un service inutilisable en envoyant une énorme quantité de demandes simultanées au service. Une possibilitée est d'utiliser plusieurs ordinateurs hacké par l'attaquant qui agissent ensemble.
Les attaquants envoient en fait une grande quantité de trafic/données falsifié à partir de différentes sources pour saturer le site ou le service qui est ciblé.

![DDoS example](https://images.frandroid.com/wp-content/uploads/2023/03/attaque-ddos-1200x675.jpg)

### Plus en détail
Ces attaques ont pour cible les infrastructures informatiques vulnérables et surchargent les serveurs et réseaux avec un trafic très élevé. Elles peuvent avoir différents formes comme les attaques volumétriques, qui innondent la cible avec un haut débit de données ou des attaques "d'épuisdement des ressources", qui visent à consommer toutes les ressources disponibles du système (CPU, upload...), ou encore des attaques sur des applications qui vont dans ce cas exploiter les failles spécifiques d'une app web.

![Surcharge trafic](https://cf-assets.www.cloudflare.com/slt3lc6tev37/7xwaGxGINeyxavVbrXO6M1/24f139faac6094044adaa84c82394962/ddos_attack_traffic_metaphor.png)

Pour mener une attaque DDoS, les attaquants utilisent souvent des botnets, c'est des réseaux d'ordinateurs infectés/hack qui sont contrôlés à distance.
Ces ordinateurs souvent obtenus par l'aide de logiciels malveillants, agissent sous le contrôle de l'attaquant sans que leur propriétaire ne s'en rende forcément compte. Les botnets permettent aux attaquants de mettre en place de manière coordonnée les attaques à grande échelle, ce qui donne plus de mal à la cible de détecter et contrer l'attaque

Les mesures de protection contre les attaques DDoS demandent en général l'utilisation d'un pare-feu, un systèmes qui va détecter les intrus (système d'intrusion), des filtres de trafic...
Les fournisseurs de services Internet (alias FSI) et les hébergeurs peuvent aussi mettre en place des protections au niveau du réseau pour détecter et bloquer le trafic malveillant.

En conclusion, une attaque DDoS est une cyberattaque qui vise à rendre un site web ou un service online indisponible en noyant ses ressources avec un trafic excessif qui vient de plusieurs ordinateurs infectés. Ces attaques exploitent les vulnérabilités de systèmes et elles peuvent causer des problèmes importants. La protection contre le DDoS nécessite des mesures de sécurité assez élevé pour garantir la disponibilité des services.



### Objectif & conséquences
Les attaques DDoS peuvent avoir des conséquences négatives pour les organisations ciblées par l'attaquant. Ce que je veux dire par là c'est que les entreprises peuvent avoir des pertes de revenus, des atteinte à la réputation car si une entreprise de sécurité est DDoS elle serait directement moins bien vue que les autres tandis qu'un entreprise qui va résister à une attaque DDoS sera alors mise en avant, une perturbation des ventes qui frustrait les utilisateurs. Elles peuvent être lancées par des individus malveillants, des groupes organisés ou des organisations criminelles.

Les attaques DDoS peuvent être utilisées à différentes fins, voici quelques raisons possibles d'une attaque DDoS:

* La concurrence:
Un attaquant peut DDoS pour se débarrasser de la concurrence, c'est donc dû à des raisons financières. En mettant en œuvre une attaque DDoS, il empêche le site Web des concurrents de faire des affaires. 
Une entreprise peut aussi se servir d’une attaque DDoS pour diminuer grandement l’activité d’une entreprise ou un magasin concurrent, et le couler financièrement.

* Raisons politiques:
Cela peut être peut-être parce que l’attaquant n’aime pas une organisation politique et veut endommager son serveur. 
On retrouve aussi les attaques DDoS dans des actions politiques. Les "hacktivistes" peuvent utiliser l’attaque DDoS pour "punir" à leur manière un service d’état ou couper la communication d’un groupe adverse.
Enfin, certains pays utilisent l’attaque DDoS pour contraindre les services d’un autre pays, et donc de cette manière déstabiliser l’action du gouvernement qui est ciblé. 
Aux tout début de la guerre en Ukraine, la Russie a été accusée d’avoir lancé beaucoup d'attaques DDoS contre les services administratifs et bancaires ukrainiens.(21)

* Chantage financier:
Un chantage financier peut être lancé par une organisation criminelle contre la promesse de ne pas paralyser des services d'une entreprise ou autre.

* Leurre:
L’attaque DDoS sert parfois de leurre à une autre attaque, comme p.ex l’installation d’un ransomware/malware ou le vol de données privées.

* Amusement:
Il se peut aussi qu’un attaquant le fasse parce qu’il le trouve drôle ou alors c'est simplement une façon de faire passer le temps.


## Techniques

Avant de comprendre comment les différentes attaques DDoS fonctionnent, il faut savoir comment la connexion réseau est établie.
Le modèle OSI, ci-dessous, _"est un cadre conceptuel utilisé pour décrire la connectivité réseau en sept couches distinctes."_ (4)

![OSI](https://cf-assets.www.cloudflare.com/slt3lc6tev37/6ZH2Etm3LlFHTgmkjLmkxp/59ff240fb3ebdc7794ffaa6e1d69b7c2/osi_model_7_layers.png)


Les attaques DDoS sont devenues plus sophistiquées au fil du temps, en utilisant des techniques comme l'amplification du trafic, l'utilisation de botnets, etc...

### Attaques sur la couche d'application
Objectif de l'attaque :

Parfois appelées attaques DDoS couche 7, ces attaques ont pour but d'épuiser les ressources de la cible pour créer un déni de service.

Les attaques visent la couche où les pages Web sont créees sur le serveur et données en réponse aux requêtes HTTP. 
Une requête HTTP ne coûte pas cher à exécuter du côté client, mais elle peut être cher pour le serveur cible qui pour y répondre doit souvent charger plusieurs fichiers et exécuter des requêtes de base de données afin de créer la page web.

Ca n'est pas simple de se défendre contre les attaques visant la couche 7 car il peut être compliqué de différencier le trafic malveillant du trafic habituel.

![Http flood](https://cf-assets.www.cloudflare.com/slt3lc6tev37/3jlyZeWRy9eBz3tyEk9mxA/96eab064524495e8f6b2647f1f7b9d60/application_layer_ddos_example.png)

Exemple:
* HTTP flood: 
Cette attaque est un peu comme si on actualiserait sans cesse un naviguateur web sur un grand nombre d'ordinateur à la fois. Cela donne lieu à un grand nombre de requête et submerge le serveur ce qui provoque un DDOS.

Les mises en œuvre les plus simples peuvent accéder à une URL avec la même plage d'adresses IP, de référents et d'agents utilisateurs malveillants. Les versions plus compliquées peuvent utiliser énormément d'adresses IP malveillantes et cibler des URL random.

### Attaques protocolaires
Les attaques protocolaires exploitent des faiblesses des couches 3 et 4 pour rendre la cible inaccessible.
Aussi appelées "attaques par épuisement des tables d'état", elles créent une interruption de services en consommant trop de ressources des serveurs ou les ressources comme le pare-feu.

![SYN FLOOD](https://cf-assets.www.cloudflare.com/slt3lc6tev37/38KdcqNuv0l0jF4ohUI7bj/44a3f60c5352984258f72a1e69e1bbdd/syn_flood_ddos_example.png)
Exemple:
* SYN flood:
_"Dans le cas d'une attaque SYN Flood la situation est comparable à celle d'un employé qui travaille dans la réserve et qui reçoit les demandes du comptoir du magasin.
L'employé reçoit une demande, va chercher le paquet et attend une confirmation avant de le transmettre au comptoir. Il reçoit ensuite de nombreuses autres demandes de colis sans confirmation jusqu'à ce qu'il ne puisse plus transporter aucun colis, qu'il soit débordé et que les demandes commencent à rester sans réponse."_ (4)
Elle permet l'envoie à une cible d'un grand nombre de paquet SYN de requête d'initialisation de connexion TCP avec des IP sources illégales. La machine ciblée va alors répondre à chaques demande de connexion et ensuite attendre la dernière étape qui n'a jamais lieu à cause du grand nombre de demande d'initialisation.

### Attaques volumétriques
Les attaques volumétriques utilisent des botnets qui sont créés à partir de plusieurs systèmes infecté par des malwares. Ils sont donc contrôlé par un attaquant et des bots sont utilisés pour DDoS la cible avec un trafic qui sature toute la bande passante disponnible.
![Aplification DNS](https://cf-assets.www.cloudflare.com/slt3lc6tev37/1FIBEeoyzoa64lVGlWKaRV/3b878bb03df1729b48cd3f667cdfe3de/amplification_ddos_example.png)



## Mécanismes de protection contre les attaques DDoS
Il est important de mettre en place des mesures/mécanismes de protection contre les attaques DDoS pour prévenir ces problèmes. 
Cela inclut la configuration correcte des infrastructures réseau, la surveillance du trafic, l'utilisation de pare-feu et de systèmes de filtrage du trafic.

Filtrage du trafic :
Le filtrage du trafic est une technique souvent utilisée pour détecter et bloquer les attaques DDoS. Il consiste à analyser le flux de données entrant et filtrer les paquets en fonction de critères défini par l'entreprise. 
Les filtres peuvent être basés sur des adresses IP, des protocoles ou même des ports. Les solutions de filtrage du trafic peuvent être mis en place au niveau du réseau, des serveurs ou des dispositifs de protection DDoS montés par l'entreprise si besoin. Les black lists et white lists sont aussi utilisées pour autoriser ou bloquer certaines adresses IP ou plages d'IP en fonction de leur réputation.

Limitation de bande passante :
La limitation de bande passante permet de contrôler la quantité de trafic permis sur un réseau ou un système. Cette technique permet de réduire l'impact des attaques DDoS en limitant la bande passante allouée à certains types de trafic comme par exemple les requêtes qui proviennent de sources suspectes ou les trop grosses requêtes.

Pare-feu :
Les pare-feu jouent un grand rôle dans la protection contre les attaques DDoS car ils surveillent et contrôlent le flux de trafic entre les réseaux. 
Ils peuvent eux aussi être configurés pour bloquer les connexions suspectes ou les requêtes excessives liées aux attaques DDoS. Les pare-feu utilisent des "règles de filtrage "pour autoriser/bloquer le trafic en fonction de critères (adresses IP source, ports, protocoles ou motifs de trafic).

Équilibrage de charge :
C'est une technique pour répartir la charge du trafic sur plusieurs serveurs. En répartissant équitablement les requêtes entrantes, ca réduit la vulnérabilité d'un serveur seul face à une attaque DDoS.

Distribution géographique des serveurs :
"La distribution géographique des serveurs" consiste à déployer des serveurs dans différentes régions géographiques. En répartissant les serveurs dans des endroits différents, les attaques DDoS qui ciblent un seul emplacement ont moins de chance de réussir. De plus, cette approche permet de réduire la latence pour les utilisateurs éloignés (Cours théorique Dev3).

## Exemples réels

Les attaques DDoS ont eu un impact sur de nombreuses organisations ou entreprises et services en ligne. Quelques exemples concret d'attaques DDoS et les mesures de protection qui ont été mises en place :

* Attaque DDoS contre GitHub (en 2018) :
    GitHub a subis une technique d'amplification basée sur le protocole Memcached pour générer un trafic massif vers les serveurs de GitHub. 
    L'attaque a atteint un pic  de 1,35 térabits par seconde (Tbps), ce qui a rendu les services de GitHub indisponibles pendant plusieurs minutes. Même si ca peut parraître minime, plusieurs minutes multiplié par le nombre d'utilisateurs c'est finalement assez énorme.
    Pour se défendre contre cette attaque, GitHub a rapidement réagi en mettant en place un système de filtre du trafic et en travaillant en collaboration avec les fournisseurs de services Internet pour filtrer le trafic malveillant. Ils ont aussi mis en place une surveillance continue pour détecter et diminuer grandement les futures attaques.

* Attaque DDoS contre Dyn DNS (en 2016) :
    Cette attaque a ciblé les serveurs DNS de Dyn qui est un important fournisseur de services DNS et a perturbé l'accès à de nombreux sites Web connus tels que Twitter, Reddit et Netflix. L'attaque a été réalisée en exploitant des milliers d'appareils IoT mal sécurisés, tels que des caméras de surveillance, pour inonder les serveurs DNS de requêtes. Les mesures de protection mises en place après cette attaque par Dyn ont inclus:
    l'amélioration de la sécurité des appareils IoT et la mise en place de mécanismes de détection des flux de trafic anormaux et l"a diversification des infrastructures DNS pour réduire l'impact potentiel d'attaques futures."

Ces exemples d'attaques DDoS mettent en avant l'importance de la réactivité et la miste en place de protection solide contre les attaques extérieures. 

## Conclusion
En conclusion, les attaques DDoS représentent une menace pour les organisations/entreprises et les services en ligne. Il est très important de mettre en place des mécanismes de protection fonctionnels et robustes (section "Mécanismes de protection contre les attaques DDoS" ci dessus). 
De plus, la préparation est un aspects clé pour contrer les attaques DDoS.

Je pense pour ma part qu'il est important de rester à jour avec les dernières techniques d'attaque et de défense contre les attaques DDoS car les attaquants malveillant continuent d'évoluer leurs stratégies. 

## Bibliographie
1. Akamai ,[ Qu'est-ce qu'une attaque DDoS ? ](https://www.akamai.com/fr/glossary/what-is-ddos), consulté le 24/05/2023
2. Kaspersky ,[Qu'est-ce qu'une attaque DDoS ?](https://www.kaspersky.fr/resource-center/threats/ddos-attacks), consulté le 24/05/2023
3. Mathilde Saliou ,[C'est quoi, une attaque collective par saturation de service ?](https://www.numerama.com/cyberguerre/1099384-quest-ce-quune-attaque-ddos.html),  25 septembre 2022, consulté le 24/05/2023
4. Cloudfare ,[Comment fonctionne une attaque DDoS](https://www.cloudflare.com/fr-fr/learning/ddos/what-is-a-ddos-attack/), consulté le 25/05/2023
5. One ,[Qu’est-ce que le DDoS ?](https://www.one.com/fr/securite-de-site-web/qu-est-ceque-attaques-force-brute-ddos), consulté le 24/05/2023
6. Frandroid ,[Qu’est-ce qu’une attaque DDoS ?](https://www.frandroid.com/culture-tech/securite-applications/1628765_quest-ce-quune-attaque-ddos), 22 mai 2023, consulté le 24/05/2023
7. Combell ,[Qu’est-ce qu’une attaque DDoS, et comment puis-je m’en protéger ?](https://www.combell.com/fr/blog/comment-proteger-votre-site-web-contre-les-attaques-ddos/), 12 mai 2021, consulté le 24/05/2023
8. ANSSI ,[Comprendre et anticiper les attaques DDoS](https://www.ssi.gouv.fr/uploads/2015/03/NP_Guide_DDoS.pdf), consulté le 24/05/2023
9. Christophe Auberger ,[Comment se protéger des attaques DDoS](https://www.journaldunet.com/solutions/dsi/1150228-comment-se-proteger-des-attaques-ddos/),  17 février 2015, consulté le 24/05/2023
10. Ahona Rudra ,[Mesures à prendre pour prévenir les attaques DDoS](https://powerdmarc.com/fr/steps-to-prevent-ddos-attacks/), 29 septembre 2022, consulté le 24/05/2023
11. AfricaCyberMag ,[Comprendre et se protéger des DDoS](https://cybersecuritymag.africa/index.php/comprendre-et-se-proteger-des-ddos), 21 avril 2023, consulté le 24/05/2023
12. Cloudflare ,[Famous DDoS attacks | The largest DDoS attacks of all time](https://www.cloudflare.com/learning/ddos/famous-ddos-attacks/), consulté le 25/05/2023
13. Lily Hay Newman ,[GitHub Survived the Biggest DDoS Attack Ever Recorded](https://www.wired.com/story/github-ddos-memcached/), 1 mars 2018, consulté le 25/05/2023
14. Wikipedia ,[Cyberattaque de 2016 contre Dyn](https://fr.wikipedia.org/wiki/Cyberattaque_de_2016_contre_Dyn), 25 mai 2022, consulté le 25/05/2023
15. Jérôme Cartegini ,[GitHub subit une attaque DDoS massive sans précédent](https://www.lesnumeriques.com/informatique/github-subit-attaque-ddos-massive-sans-precedent-n72155.html), 6 mars 2018, consulté le 25/05/2023
16. Fabrice Clerc ,[Retour sur l’attaque DDoS contre DYN DNS](http://www.mtom-mag.com/article2905.html), octobre 2016, consulté le 25/05/2023
17. Jean Elyan ,[10 choses à savoir sur les attaques DDoS massives contre Dyn](https://www.lemondeinformatique.fr/actualites/lire-10-choses-a-savoir-sur-les-attaques-ddos-massives-contre-dyn-66325.html), 25 octobre 2016, consulté le 25/05/2023
18. Wikipedia ,[Cloudflare](https://fr.wikipedia.org/wiki/Cloudflare), 16 septembre 2022, consulté le 25/05/2023
19. DataNews ,[Cloudflare enregistre le plus grande attaque DDoS à ce jour](https://datanews.levif.be/actualite/cloudflare-enregistre-le-plus-grande-attaque-ddos-a-ce-jour/), 14 février 2022, consulté le 25/05/2023
20. OVH ,[ Qu’est-ce qu’une attaque DDoS ? ](https://www.ovhcloud.com/fr/security/anti-ddos/ddos-definition/), consulté le 25/05/2023
21. Steve Holland , [ Russia responsible for cyberattack against Ukrainian banks](https://www.reuters.com/world/us-says-russia-was-responsible-cyberattack-against-ukrainian-banks-2022-02-18/), 18 février 2022, consulté le 25/05/20223