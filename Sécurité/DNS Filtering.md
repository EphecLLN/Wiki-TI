---
layout: default
title: DNS Filtering
parent: Sécurité
---
# DNS Filtering

## Introduction

Le DNS Filtering (ou filtrage DNS en français) est une technique utilisée pour contrôler et restreindre l'accès à Internet en fonction des noms de domaine demandés par les utilisateurs. Le DNS, qui signifie Domain Name System, est le système utilisé sur Internet pour traduire les noms de domaine en adresses IP, permettant ainsi aux utilisateurs d'accéder aux sites web en utilisant des noms conviviaux plutôt que des séries de chiffres.

Le filtrage DNS s'appuie sur cette infrastructure pour bloquer, rediriger ou autoriser sélectivement les requêtes DNS en fonction de règles préétablies. Il peut être utilisé à différents niveaux, que ce soit au niveau de l'utilisateur individuel, du réseau domestique, de l'entreprise ou même d'un pays entier.

## Fonctionnement du DNS Filtering [1][2][3][4][5] 

Les requêtes DNS sont dirigées vers un résolveur DNS. Certains résolveurs DNS peuvent agir comme des filtres en refusant de résoudre les requêtes pour certains domaines répertoriés dans une liste de blocage, empêchant ainsi les utilisateurs d'accéder à ces domaines. Le filtrage DNS peut également utiliser une liste d'autorisations au lieu d'une liste de blocage.

Démontrons par un exemple pour mieux comprendre le fonctionnement du filtrage DNS : supposons qu'un employé reçoive un e-mail de phishing et clique sur un lien menant à malware-website.com. Avant que l'ordinateur de l'employé ne charge le site Web, il envoie une requête au service de résolution DNS de l'entreprise qui utilise le filtrage DNS. Si le site malveillant est présent dans la liste de blocage de l'entreprise, le résolveur DNS bloquera la demande, empêchant ainsi le chargement de malware-website.com et contrant l'attaque de phishing.

Le filtrage DNS peut bloquer les sites Web soit par leur nom de domaine, soit par leur adresse IP :

* Par domaine : le résolveur DNS n'effectue pas de recherche ou de résolution d'adresses IP pour certains domaines spécifiques.
* Par adresse IP : le résolveur DNS tente de résoudre les domaines pour toutes les adresses IP, mais s'il trouve une adresse IP figurant sur la liste noire, il ne la renverra pas à l'appareil qui a effectué la demande.

## Qu'est-ce qu'une liste de blocage ? [1]

Dans le DNS Filtering, une liste de blocage est une compilation de domaines ou d'adresses IP connus pour être nuisibles. Les fournisseurs de filtrage DNS utilisent ces listes pour empêcher l'accès à des sites dangereux. Ils peuvent se baser sur des listes partagées au sein de la communauté de la cybersécurité, créer leurs propres listes ou faire les deux. Certains filtres DNS analysent les pages Web et ajoutent automatiquement celles qui sont malveillantes à la liste de blocage. Par exemple, si un site comme example.com exécute un code JavaScript malveillant, example.com sera ajouté à la liste de blocage.

Le filtrage DNS peut également bloquer des domaines qui ne sont pas directement liés à des logiciels malveillants ou à des attaques de phishing, mais qui hébergent du contenu interdit ou inapproprié. Par exemple, une entreprise peut décider d'ajouter des sites pour adultes à sa liste de blocage de filtrage DNS.

En revanche, une liste blanche est une compilation de domaines ou d'adresses IP autorisés. Seuls les domaines ou adresses IP présents sur cette liste sont autorisés, tandis que tous les autres sont bloqués.

## Utilisation du DNS Filtering [1][3][4]

Bien que le filtrage DNS ne doive pas être la seule solution pour protéger les données de votre entreprise, il reste une mesure de sécurité très efficace.

Il y a trois principaux cas d'utilisation du filtrage DNS.

1. Bloquer les sites Web hébergeant des logiciels malveillants :
Les sites Web contenant des logiciels malveillants peuvent causer des dommages à votre entreprise. Une fois que les réseaux gérés par votre entreprise sont infectés par des logiciels malveillants, vous pouvez vous attendre à ce que vos opérations et votre productivité soient gravement perturbées. Le filtrage DNS est souvent la première ligne de défense contre de telles infections.
Les employés peuvent être induits en erreur ou redirigés vers des sites Web malveillants où des logiciels malveillants sont téléchargés sur leurs appareils. À partir de là, ces logiciels peuvent se propager dans toute l'infrastructure de l'entreprise et, s'ils ne sont pas contrôlés, finir par paralyser les opérations de l'entreprise. C'est pourquoi le filtrage DNS est une mesure judicieuse pour prévenir les violations de données de l'entreprise.

2. Bloquer les sites de phishing :
Les attaques de phishing sont courantes pour compromettre l'ensemble du réseau d'une entreprise. Dans ce cas, les attaquants essaient de tromper vos employés pour qu'ils divulguent leurs informations de connexion. La protection contre le phishing est un cas d'utilisation connexe où les attaquants tentent d'escroquer les employés en les redirigeant vers un site Web qui semble légitime et qui demande un nom d'utilisateur et un mot de passe. Ces sites ressemblent souvent visuellement aux vrais sites, mais leurs noms de domaine et adresses IP sont différents.
Le filtrage DNS agit comme un filet de sécurité pour les employés qui pourraient involontairement tomber dans le piège du phishing.

3. Bloquer l'accès aux sites Web interdits :
Vous souhaitez maximiser la productivité de votre équipe. Parfois, certains sites Web peuvent entraver cette productivité, ce qui peut avoir un impact sur votre entreprise. Vous pouvez utiliser des outils de filtrage DNS pour appliquer des politiques de contenu et de navigation sur Internet en bloquant l'accès aux adresses IP des sites Web que vous ne souhaitez pas que votre équipe visite.

## Limitations du DNS Filtering [6]

Le filtrage DNS présente certaines limites qui nécessitent l'utilisation d'autres méthodes complémentaires. Voici quelques exemples de ce qu'il ne peut pas faire ou de ses limitations :

* Le filtrage DNS fonctionne de manière similaire aux logiciels antivirus traditionnels, en s'appuyant sur une base de données généralement gérée par le fournisseur de services. Cependant, cela signifie que les utilisateurs ne seront pas protégés contre les sites récemment créés qui ne sont pas encore répertoriés dans la base de données. La réactivité du fournisseur est cruciale pour assurer une protection efficace.

* Les utilisateurs peuvent toujours accéder à des sites distrayants ou à du contenu illégal en utilisant le réseau de téléphonie mobile, contournant ainsi les filtres DNS.

* Il est possible de contourner les filtres DNS en utilisant des mesures telles que l'utilisation d'un serveur DNS public ou d'un proxy. Ces méthodes doivent être bloquées à l'aide d'un pare-feu fiable.

* Bloquer un grand nombre de sites Web peut agacer les travailleurs qui souhaitent effectuer des activités personnelles limitées et raisonnables pendant une pause ou un déjeuner.

* L'utilisation du filtrage DNS sur les appareils personnels (BYOD) peut poser des problèmes lorsqu'il s'agit d'utiliser l'appareil de manière sécurisée à des fins personnelles, voire de se connecter à un réseau personnel dans certains cas.

* Bien que les faux positifs soient rares avec le filtrage DNS moderne, ils peuvent toujours se produire. Cette limitation du filtrage DNS réside dans son blocage global au niveau du domaine. Un "faux positif" peut également indiquer une compromission du site auquel vous tentez d'accéder. Dans ce cas, il peut être difficile pour le propriétaire du site de le retirer de la liste bloquée. Si un site réputé est soudainement bloqué, il est recommandé de contacter son propriétaire pour l'en informer.

* Les cybercriminels peuvent facilement changer de nom de domaine, créant ainsi une sorte de "course aux armements" entre les fournisseurs de filtrage et les escrocs. La plupart des cybercriminels abandonnent rapidement les sites qui ont été mis sur liste noire.

* Le filtrage DNS n'est pas compatible avec DNSSEC, qui vise à protéger les utilisateurs contre les redirections malveillantes et les techniques de phishing utilisant ce que l'on appelle "l'empoisonnement DNS". Il est donc nécessaire de prendre en compte l'importance relative de ces deux aspects pour votre entreprise. Bien que le filtrage DNS soit plus utile pour de nombreuses entreprises, il est important de réfléchir à leur pertinence en fonction de votre secteur d'activité et du type de données traitées.

## Filtrage DNS vs Filtrage Web [1][5]

Il y a deux types de filtrage de contenu : le filtrage DNS et le filtrage Web. Le filtrage DNS restreint l'accès aux sites Web en se basant sur les requêtes DNS, tandis que le filtrage Web bloque l'accès à des sites Web spécifiques en fonction de leurs URL. Le filtrage DNS est souvent plus efficace que le filtrage Web, car il peut empêcher l'accès aux sites Web avant même qu'ils ne soient chargés.

En général, les filtres DNS sont plus précis que les filtres Web. Les requêtes DNS sont souvent plus précises que les URL. Par exemple, une requête DNS pour "example.com" renverra toujours la même adresse IP. En revanche, selon votre région, l'URL example.com peut changer. De plus, votre connexion peut également influencer ces changements.

Le filtrage Web prend généralement plus de temps que le filtrage DNS. Les requêtes DNS sont généralement résolues plus rapidement que les URL. De plus, le filtrage DNS peut entraver l'accès aux sites Web qui utilisent des connexions sécurisées (HTTPS).

## Conclusion

En conclusion, le DNS Filtering est une mesure de sécurité efficace pour contrôler et restreindre l'accès à Internet. Il permet de bloquer les sites malveillants, de prévenir les attaques de phishing et d'optimiser la productivité des utilisateurs. Cependant, il présente certaines limitations et doit être complété par d'autres méthodes pour renforcer la sécurité.

## Bibliographie

* [1] CloudFlare, [What is DNS filtering? | Secure DNS servers](https://www.cloudflare.com/learning/access-management/what-is-dns-filtering/), consultée le 23 mai 2023

* [2] Malwarebytes, [What is DNS filtering?](https://www.malwarebytes.com/cybersecurity/business/what-is-dns-filtering), consultée le 23 mai 2023

* [3] Cezarina Dinu, [What Is DNS Filtering and Why Does Your Business Need It?](https://heimdalsecurity.com/blog/dns-filtering/), version du 9 septembre 2022, consultée le 23 mai 2023

* [4] GoodAcces, [How DNS Filtering Can Help Secure Your Business](https://www.goodaccess.com/dns-filtering-explained-how-it-works-and-its-usage), consultée le 23 mai 2023

* [5] Beloslava Petrova, [What is DNS filtering? Do you need it?](https://www.cloudns.net/blog/what-is-dns-filtering-do-you-need-it/), version du 22 février 2022, consultée le 23 mai 2023

* [6] Paul William, [The Importance of DNS Filtering](https://www.appliedi.net/blog/the-importance-of-dns-filtering/), consultée le 23 mai 2023