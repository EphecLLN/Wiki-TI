---
layout: default
title: Installation Reverse Proxy
parent: Réseaux
---
# installation d'un reverse proxy [^1]

Il y a beaucoup de reverse proxy possibles. Pour ce tutoriel, nous allons nous concentrer sur Nginx, puisque c'est l'un des plus utilisés et qu'il a une grande versatilité. Pour la suite, nous supposerons également l'utilisation de Linux. Pour Windows voyez [ici](https://nginx.org/en/docs/windows.html)

## 0. pourquois? [^5]

La nécessité d'un reverse proxy tel que Nginx n'est pas toujours évidente. Mais si l'on prend l'exemple d'un serveur web contenant des pages statiques, voici les avantages que l'on peut observer :

- **load balancig** : Avec Nginx, il est possible de rediriger les requêtes utilisateurs vers différents serveurs, par exemple un Docker Swarm, afin de répartir la charge de trafic. Ainsi, à partir d'une seule adresse, il est possible de gérer un trafic beaucoup plus important que si le serveur web était seul.

- **cache** : Dans le scénario précédent, un trafic important de pages statiques peut grandement bénéficier d'un cache. Cependant, si le cache est géré au niveau des serveurs, il n'y a pas d'homogénéité entre les caches. Pour pallier cela, le cache est géré par Nginx.

- **terminaison SSL** : Si vous avez un Docker Swarm, il est bien plus intéressant de configurer SSL une fois sur Nginx plutôt que de le faire individuellement sur chaque conteneur.

- **sécuritée et controle d'accès**: Ceci regroupe un ensemble de fonctionnalités telles que le filtrage d'IP, un pare-feu et une détection de spam.

- **CDN** ou *content delivery network* : Un réseau de reverse proxy situé à plusieurs endroits dans le monde permet d'optimiser la diffusion de contenu. L'avantage principal est que toutes les fonctionnalités de cache sont plus proches des utilisateurs finaux, ce qui réduit considérablement la latence des applications web.

## 1. installer Nginx [^2]

Nginx supporte un grand nombre de distributions (distros). [ici](https://nginx.org/en/linux_packages.html)
Il est donc possible de l'installer avec la plupart des gestionnaires. Ici, nous utiliserons Ubuntu.
`sudo apt update`
`sudo apt upgrade`
`sudo apt install nginx`

Ensuite pour lancer ou arrêter voici les commandes
- `nginx` pour démarrer (lancez l'exe)
- `nginx -s stop` pour fermer tout de suite (à éviter)
- `nginx -s quit` pour fermer correctement
- `nginx -s reload` pour recharger la config

## 2. configuration [^3]
### structure de base
La configuration de Nginx se fait à travers des blocs imbriqués. Chaque bloc correspond à un contexte dans lequel les options spécifiées seront appliquées. Par exemple, le contexte 'http' peut être placé directement dans le fichier de configuration, tandis que le contexte 'server' doit être situé à l'intérieur d'un contexte tel que 'http' ou 'mail' pour fonctionner correctement.  

La rédaction de la configuration se fait donc par la création de ces blocs de configuration, chacun précisant le fonctionnement de son parent. voici un exemple:
```
http {
    server {
        location / {
            root /data/www;
        }

        location /images/ {
            root /data;
        }
    }
}
```
Ici, on crée un serveur HTTP qui aura deux comportements différents en fonction de la route de la requête.
Le premier définit que l'accès à la page racine et doit rediriger vers le fichier se trouvant dans "/data/www".
Alors que "/images/" renvoie directement à "data".  

### les redirections
Précédemment, le serveur se contentait de servir des pages statiques en réponse. Cependant, l'intérêt d'un reverse proxy est de déléguer la tâche de servir les pages à un ou plusieurs autres serveurs. Cela se fait avec `proxy_pass`, qui définit l'URL ou l'adresse IP ainsi que le port de destination, voici un exemple:
```
#contexte http
server {
    location / {
        proxy_pass http://localhost:8080;
    }

    location /images/ {
        root /data;
    }
}
```
Ici, on définit que les accès à `/images/` seront gérés par Nginx, tandis que le reste des routes sera redirigé vers un autre serveur tournant sur la même machine. Il est important de noter que "localhost" peut être remplacé par n'importe quelle URL ou adresse IP. Dans le cas des adresses IP, la notation se fait comme suit : `http://192.168.0.1:80`.

Notez que dans les deux cas, le préfixe "http" est présent. Il est nécessaire pour que Nginx sache quel protocole employer.

### filtre des entrée
Pour faciliter la sécurité, il est possible de choisir quels sont les ports écoutés et même les adresses IP autorisées en utilisant la directive listen, come ceci:
```
server {
    listen 127.0.0.1:8080;

    # reste de la config
}
```
Il est important de savoir que pour utiliser le port 8080, il est obligatoire d'utiliser la directive `listen`, au minimum avec `listen 8080;`, qui écoute toutes les adresses IP entrantes. Sans cela, le port 80 sera utilisé par défaut.

### routes regex
Pour faciliter la séparation des routes vers différentes destinations, Nginx supporte l'utilisation des expressions régulières (regex). Pour utiliser une route en regex, il suffit de la précéder d'un `~` ou `~*`. La différence est que le premier est sensible à la casse (case-sensitive), tandis que le second ne l'est pas, voici un exemple:
```
location ~ \.html? {
    #...
}
```
Ici toutes les requêtes se finissant par '.html' ou '.htm'. plus d'infos sur les regex [ici](https://regexr.com/)

### conservation de la requête initiale
Dans tous les exemples précédents, les en-têtes (headers) des requêtes sont remplacés par Nginx. Cela peut poser un problème si le serveur de destination a besoin de l'en-tête d'origine. Pour le conserver, deux options peuvent être utilisées:
```
location / {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;

    # reste de la config
}
```
Ici, 'host' se voit attribuer la variable `$host`, qui désigne l'hôte d'origine, contrairement à `$proxy_host`, utilisé par défaut, qui utilise le reverse proxy. De la même manière, 'x-real-ip' reçoit l'IP du client effectuant la requête.

### exemple complet
voici un exemple reprenant la plupart des techniques vue plus haut dans un fichier de configuration.
```
http {
    server {
        listen 8080;

        location / {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_pass http://127.0.0.1:8000;
        }
    }
}
```

## Bibliographie

[^1]: non mentionné, [RFC 1034 - DOMAIN NAMES - CONCEPTS AND FACILITIES](https://www.wappalyzer.com/technologies/reverse-proxies/#:~:text=Reverse%20proxies%20technologies%20market%20share%20These%20are%20the,in%202023.%20Nginx%2094.6%25%20Envoy%205%25%20Other%200.4%25), 2023
    
    **Résumé** : graph des parts de marché des différent reverse proxy
    **Avis sur la ressource** : manque de source mais un tour rapide sur Reddit corrobore la place de nginx comme plus populaire.

[^2]: non mentionné, [guide débutant Nginx](https://nginx.org/en/docs/beginners_guide.html)

    **Résumé** : guide d'installation pour Nginx par Nginx
    **Avis sur la ressource** : manque absolu de détails sur la configuration mais heureusement les autres articles comblent ce manque.

[^3]: non mentionné, [guide débutant Nginx](https://nginx.org/en/docs/http/ngx_http_proxy_module.html)

    **Résumé** : détail des options de proxy
    **Avis sur la ressource** : très complet mais manque d'exemple

[^4]: non mentionné, [guide débutant Nginx](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)

    **Résumé** : guide de config pour proxy un site web
    **Avis sur la ressource** : très clair et concis

[^5]: non mentionné, [wiki: reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy)

    **Résumé** : page wiki sur les reverse proxy
    **Avis sur la ressource** : manque de détails mais utile d'un point de vue informatif
