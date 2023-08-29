# Article Http2

## Qu’est que HTTP/2 ?

HTTP/2 est une révision majeure du protocole HTTP, qui est le protocole de couche applicative utilisé dans le web. La première version de HTTP fut HTTP/0.9, qui proposait uniquement de faire des requêtes GET. Il est ensuite suivi de HTTP/1 puis HTTP/1.1 (1997) puis vient HTTP/2.

## Histoire

Google a développé un protocole de remplacement à HTTP/1.1 nommé SPDY (speedy) qui avait pour objectif d’augmenter la vitesse par rapport à celui-ci. En 2012, le projet initial de HTTP/2 est développé et se base directement sur SPDY.  En 2015, l’IESG (Internet Engineering Steering Group) qui est chargé des standards d’internet approuve la norme HTTP/2. Par la suite, le navigateur Google Chrome annonce l'abandon progressif de SPDY au profit d’HTTP/2.

## Objectif


Le principale objectif de HTTP/2 est de réduire la latence (c’est pour ça que celui-ci a pris comme base le protocole SPDY). En effet, les pages web d’aujourd’hui ont grandement évolué par rapport à celles qu’on trouvait au début de HTTP/1.1, celles-ci devant générer des centaines de requêtes. Hors, HTTP/1.1 ne supporte qu’une requête par connexion.Pour corriger ce problème, HTTP/2 autorise lui qu’une connexion par site mais il permet de transmettre plusieurs requêtes par messages (pipeline). 


## Changement par rapport à HTTP/1.1

Une connexion TCP multiplexé: Comme précisé au dessus, HTTP/2 ne permet qu’une connexion TCP car celle-ci prend beaucoup de temps à s’établir, mais permet de faire une requête sur cette même connexion.
Compression d’en-tête HTTP: Contrairement à HTTP/1.1, HTTP/2 compresse les en-têtes. De plus, il ne place un en-tête que s' il y a un changement d’état par rapport à la requête précédente, réduisant ainsi la répétition.
Serveur Push: Au lieu de lire la page HTML et de déterminer les ressources nécessaires à partir de celle-ci (exemple: page de style css), le serveur se charge de les envoyés directement, évitant ainsi d'attendre que le serveur lise la page HTML

![image](https://i.goopics.net/jof53p.png)

source: https://www.infoq.com/fr/articles/http2-les-details/


## Avantages de HTTP/2

Performance: le multiplexage permet de transmettre plus de données en même temps.
Performances du web mobiles: le multiplexage et la compression d’en-tête permet de diminuer la latence sur les réseaux mobiles.
Diminution des coûts: l’augmentation du débit obtenue avec HTTP/2 permettra aux services de télécommunication de réduire leur dépense, réduisant ainsi les tarifs proposés par les fournisseurs.
Sécurité: L'algorithme HPACK (pris en charge par HTTP/2) permet de contrer les menaces de sécurité.

## Place de HTTP/2 aujourd’hui:
En 2021, 47% des sites webs les plus connus utilisaient HTTP/2. 97% des navigateurs pouvaient prendre en charge HTTP/2 fin 2015.

## Conclusion
Le web ne cesse d’évoluer ces dernières années, à mesure que les utilisateurs et les sites se font de plus en plus nombreux. Le protocole applicatif HTTP se doit donc d’évoluer pour répondre à l'expansion du web. HTTP/2 est une grande évolution de ce protocole qui permettra aux utilisateurs d’internet d’avoir une meilleure expérience avec un web plus rapide, moins coûteux et sécurisé. 







Bibliographie

https://en.wikipedia.org/wiki/HTTP/2

https://developer.mozilla.org/fr/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP

https://fr.wikipedia.org/wiki/Internet_Engineering_Steering_Group

https://web.dev/performance-http2/

https://stileex.xyz/pourquoi-internet-rapide-http2/

https://boowiki.info/art/hypertext-transfer-protocol/http-2.html#Obiettivi

https://blog.dareboost.com/fr/2016/11/http2-changements-bonnes-pratiques-http1-front-end/

https://kinsta.com/fr/apprendre/http2/#main_benefits_of_http2
