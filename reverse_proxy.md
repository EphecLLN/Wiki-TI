[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# Le Reverse Proxy
Dans cet article, nous allons essayer de répondre à la question  
**"Qu'est-ce qu'un reverse proxy et pourquoi en utiliser un ?"**
## Tout d'abord, qu'est-ce qu'un proxy ?
En résumé, un proxy (aussi appélé "*forward proxy*" en anglais [4]) est un logiciel ou une machine permettant de sécuriser une infrastructure réseau via différents points. Il se place juste après les clients de sorte à recevoir leurs requêtes avant qu'elles ne sortent du réseau et qu'elles n'atteignent Internet.

![proxy](https://www.ionos.fr/digitalguide/fileadmin/DigitalGuide/Schaubilder/Forward_Proxy.png)
*Représentation schématique d’un proxy [4]
https://www.ionos.fr/digitalguide/fileadmin/DigitalGuide/Schaubilder/Forward_Proxy.png*  

Il existe différent types de Proxy:  
- **Proxy DNS**: sert pour un "accès à la résolution DNS Internet via le résolveur externe[3]"
- **Proxy Web**: permet d'accélérer la navigation en gérant la mémoire cache et en assurant la compression des données. Il sécurise aussi la navigation en bloquant certains sites ou publicités[1]. On peut aussi mettre un système d'authentification pour atteindre des ressources extérieures. 

**Exemple d'utilisation d'un proxy:**  
Une entreprise utilisant un proxy pour bloquer des ressources jugées inutiles pour l'entreprise (ex: réseaux sociaux, pornographie, ...).[2]



## Mais du coup, qu'est-ce qu'un reverse proxy ?

Un reverse proxy (ou en français un proxy inverse [5]) est un logiciel ou une machine qui se place du côté opposé à celui du proxy ce qui veut dire qu'il se place à l'entrée d'un réseau, juste avant de joindre un serveur (Web par exemple).  

![proxy](https://www.ionos.fr/digitalguide/fileadmin/DigitalGuide/Schaubilder/Reverse_Proxy.png)
*Représentation schématique d’un reverse proxy [4]
https://www.ionos.fr/digitalguide/fileadmin/DigitalGuide/Schaubilder/Reverse_Proxy.pngg*  

Un utilisateur passera par le reverse proxy pour accéder aux services internes. De cette façon, il permet de rajouter une sécurité supplémentaire et protége les serveurs des attaques potentielles venant de l'exterieur.  

Quand le proxy reçoit une requête venant d'Internet, il regarde d'abord si cette dernière est conforme aux règles de sécurité présentent dans la configuration du reverse proxy.  

Finalement, le reverse proxy permet aussi d'optimiser le trafic des données d'un réseau via différentes manières.

## Quelles sont les applications que peut avoir un reverse proxy ?
Le reverse proxy est utilisé dans plusieurs buts bien distincts.
### Le reverse proxy permet le caching d'information
Le reverse proxy peut stocker en mémoire une ou plusieurs pages HTML statiques (uniquement avec un contenu qui ne change pas en fonction de l'utilisateur qui l'utilise).  

Quand une requête pour une page statique, qu'il avait précédement stockée en mémoire, arrive, le reverse proxy lui fournit la page sans même faire une seule requête au serveur Web.  

De cette façon, la charge des serveurs Web est globalement diminuée. Cette utilisation-ci s'appelle « accélérateur web » ou « accélérateur HTTP »[5] ou encore le caching[4].  

De temps en temps, le reverse proxy ira demander au serveur si les données qu'il a stocké son toujours à jour ou si le contenu des pages ont changés. Si ce n'est plus le cas, il demande les nouvelles pages au serveur Web et il remplace les données dans sa mémoire cache.  

De cette façon, le reverse proxy est sûr de rester à jour.

### Le reverse proxy permet l'anonymisation des serveurs
Vu que toutes les requêtes passent par le reverse proxy qui transfère la demande au service demandé, les serveurs restent anonymes ce qui rajoute une sécurité non négligable.

### Le reverse proxy permet la répartition de charge
La répartition de charge ou "*load-balancing*" en anglais. On entend beaucoup ce terme dans les manuels d'école ou dans le milieu de l'informatique mais qu'est-ce que ça veut vraiment dire ?  

Il n'est pas rare dans une entreprise d'avoir plusieurs serveurs (Web par exemple) de telle façon que si un serveur devient inaccessible pour une raison ou une autre (panne, surcharge,...), le service n'est pas interrompu car un autre serveur prend la relève. Ce système est appelé "**répartition de charge**".  

Oui mais le reverse proxy dans tout ça ? Il permet tout simplement de pouvoir, quand il reçoit une requête, choisir vers quel serveur la diriger et donc de répartir la charge de travail sur plusieurs serveurs pour les surcharger le moins possible.  

Oui mais comment peut-il faire cela ? Tout simplement en liant une URL pour différents serveurs[4].

### Le reverse proxy permet la compression des données
Pour un échange plus rapide, le reverse proxy peut compresser les données avant de les envoyer au client.  

Pour faire cela, le reverse proxy va avoir besoin d'un logiciel supplémentaire. Le plus connu pour compresser les sites Web est Gzip lequel est souvent utilisé avec Apache ou nginx [4].

### Le reverse proxy permet l'encodage et le contrôle des données
On peut installer, sur un reverse proxy, des antivirus ou des filtres de paquet pour analyser plus en profondeur les paquets reçus, ce qui permet de sécuriser les paquets qui atteignent les serveurs.  

L'encodage des données est aussi possible grâce à des outils externes.

### Le reverse proxy permet de surveiller l'envoi des paquets
Si un serveur ne traite pas bien une demande à cause d'une erreur quelconque, une erreur est renvoyée. Sauf que certaines erreurs ne doivent pas être visibles par le client.  

Le reverse proxy se charge donc de filtrer les paquets sortant du réseau (typiquement des réponses des différents serveurs) et d'arrêter certains paquets contenant, justement, ces fameuses erreurs. Il peut aussi stopper l'envoi d'information considéré comme désirée comme les tokens ou autres [6].

### Le reverse proxy permet une surveillance facilitée pour l'administrateur
Vu que toutes les requêtes passent par le reverse proxy, en consultant ses logs l'administrateur système peut voir les requêtes qui ont été bloqués et voir si elles sont justifiées.  

Si elles ne le sont pas, il peut très facilement changer les règles de filtrage directement sur la même machine. 

## Donc l'implémentation d'un reverse proxy résout tout les problèmes de sécurité ?
Pas tout à fait ! Tout d'abord, pour qu'un reverse proxy soit utilisé comme il faut, il doit disposer de WAF ([Web Application Firewalls](https://www.vaadata.com/blog/fr/les-3-configurations-des-waf-pour-se-proteger-des-cyber-attaques/)).

Ensuite, une mise à jour des listes noires (contenu bloqué) et des listes blanches (contenu autorisé) doit être faite assez fréquement pour éviter que des demandes valides ne soient bloquées et donc, que cela gache l'expérience utilisateur.  

Pour finir, la mise à jour des listes noires est impérative pour garantir d'être à jour pour contrer les nouvelles menaces, auquel cas, le reverse proxy deviendra de plus en plus obsolète et deviendra inutile.

## En conclusion
Un reverse proxy est très intéressant pour protéger les serveurs et pour filtrer les requêtes les atteignant. Toutefois, une mauvaise configuration ou une mauvaise mise à jour des données pourrait le rendre inutile ou pire, pourrait gâcher l'expérience utilisateur et être négatif pour l'entreprise.

***Article écrit par Kevin Keurvels HE201913.***

## Bibliographie 
* [Proxy - Wikipédia](https://fr.wikipedia.org/w/index.php?title=Proxy&oldid=193004402), La communauté mais validé par des administrateurs, 20 avril 2022, consulté le 03 mai 2022
   - Résumé : Page expliquant ce qu'est un proxy dans toutes ses utilisations
   - Avis sur la ressource : Ressource assez complète et qui à l'air sérieuse

* [Qu'est ce qu'un proxy ?](https://www.commentcamarche.net/faq/17453-qu-est-ce-qu-un-proxy), Jean-François Pillou, 28 février 2018, consulté le 03 mai 2022
   - Résumé : Ressource assez basique expliquant ce qu'est un proxy et son principe de fonctionnement
   - Avis sur la ressource : La ressource explique bien les principes fondamentaux d'un proxy 
* [Slides : DNS avancé](https://moodle.ephec.be/pluginfile.php/369478/mod_resource/content/0/Admin-Res-Sys-II_4_DNS.pdf), Virginie Van den Schrieck, date de création inconnue, consulté le 3 mai 2022
   - Résumé : Powerpoint expliquant plein de concepts avancés sur le DNS dont le proxy dans l'aspect sécurité 
   - Avis sur la ressource : Ressource très intéréssante et assez complète
* [Le serveur reverse-proxy : Eléments principaux](https://www.ionos.fr/digitalguide/serveur/know-how/quest-ce-quun-reverse-proxy-le-serveur-reverse-proxy/), l'auteur est inconnu, 17 mars 2020, consulté le 4 mai 2022
   - Résumé : Article expliquant les principes du reverse proxy et qui dit comment en configurer un
   - Avis sur la ressource : Ressource assez sérieuse, professionnelle et trés détailée
* [Proxy inverse - Wikipédia](https://fr.wikipedia.org/w/index.php?title=Proxy_inverse&oldid=186198908e), La communauté mais validé par des administrateurs, 9 septembre 2021, consulté le 4 mai 2022
   - Résumé : Page expliquant de manière assez brève ce qu'est un reverse proxy tout en détaillant ses applications
   - Avis sur la ressource : Ressource utile pour avoir une idée générale sur le sujet sans trop rentrer dans les détails. 
* [Improve Your Application Security Using a Reverse Proxy](https://traefik.io/blog/improve-application-security-using-a-reverse-proxy/), Manuel Zapf, 13 janvier 2022, consulté le 4 mai 2022
- Résumé : Article en anglais centré sur les aspects sécurités qui justifient l'utilisation d'un reverse proxy
   - Avis sur la ressource : Article très complet et rempli d'informations utiles
* [Les 3 configurations des WAF pour se protéger des cyber-attaques](https://www.vaadata.com/blog/fr/les-3-configurations-des-waf-pour-se-proteger-des-cyber-attaques/), Auteur inconnu, 11 mars 2015, consulté le 5 mai 2022
   - Résumé : Article expliquant ce qu'est un WAF et comment en utiliser un.
   - Avis sur la ressource : Ressource utile et compréhensible pour tout un chacun. Il informe bien sur les WAF.

   
   

