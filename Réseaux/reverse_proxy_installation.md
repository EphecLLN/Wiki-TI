---
layout: default
title: Installation Reverse Proxy
parent: Réseaux
---
# installation d'un reverse proxy [^1]

il y a beaucoup de proxy possibles. pour ce tutoriel nous allons nous concentrer sur Nginx. puisque c'est l'un des plus utilisé et qu'il a un grande versatilité. Pour la suite, nous supposeront également l'utilisation de Linux. Pour Windows voyez [ici](https://nginx.org/en/docs/windows.html)

## 1. installer Nginx [^2]

Nginx supporte un grand nombre de distros [ici](https://nginx.org/en/linux_packages.html)
il est donc possible de l'installer avec la plupart des package manager.
`sudo apt update`
`sudo apt upgrade`
`sudo apt install nginx`

ensuite pour lancer ou arrêter voici les commandes
- `nginx` pour démarrer (lancez l'exe)
- `nginx -s stop` pour fermer tout de suite (à éviter)
- `nginx -s quit` pour fermer correctement
- `nginx -s reload` pour recharger la config

## 2. configuration [^3]
### structure de base
la configuration de nginx se fait par 'block' imbriqué. chaque block correspond a un contexte pour lequel les option écrite a l'intérieur seront appliqué. par exemple voici le contexte 'HTTP' peut être placé directement dans le fichier de config alors que 'server' doit être dans un contexte tel que 'http' ou 'mail' pour fonctionner.

la rédaction de la config se fait donc par la création de ces block de config chacun précisent le fonctionnement de leurs parent. voici un exemple:
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
ici on crée un serveur http qui aura deux comportements différent en fonction de la route de la requête.
le premier définit que l'accès a la page racine doit rediriger vers le fichier se trouvant dans /data/www. alors que /images/, lui, revoie directement dans data.  

### les redirections
précédemment, le serveur se contentait de servir des pages statiques en réponse. mais l’intérêt d'un proxy est de déléguer la tache de servir les pages a un ou même plusieurs autres serveurs. cela se fait avec `proxy_pass`, qui définit l'url ou ip et le port de destination voici un exemple:
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
ici on définit que les accès a `/images/` seront géré par nginx alors que le reste des routes sera redirigé vers un autre serveur tournant sur la même machine. il est important de noté que le 'localhost' peut être remplacé par n'importe quel url ou ip. dans le cas des ip la notation ce fait comme suis `http://192.168.0.1:80`.

Notez que dans les deux cas le préfixe 'http' est présent, il est nécessaire pour que nginx sache quel protocole employer.

### filtre des entrée
pour faciliter la sécurité, il est possible de choisir quel sont les ports écouté et même les ip autorisée. pour cela il suffi d'utiliser `listen`. come ceci:
```
server {
    listen 127.0.0.1:8080;

    # reste de la config
}
```
Il est important a savoir que pour utiliser le port 8080, il est obligatoire d'utiliser listen, au minimum avec `listen 8080;` qui écoute toutes les ip entrantes. sans cela, le port 80 sera utilisé par défaut.

### routes regex
pour faciliter la séparation des routes vers différentes destinations, nginx supporte l'utilisation des regex. pour utiliser une route regex il suffit de la précéder d'un `~` ou `~*`. la différence est que le premier est 'case-sensitive' et le second non. voici un exemple:
```
location ~ \.html? {
    #...
}
```
ici toutes les requêtes se finissant par '.html' ou '.htm'. plus d'infos sur les regex [ici](https://regexr.com/)

### conservation de la requête initiale
Dans tout les exemple précédents, les header des requêtes sont remplacé par nginx. C'est un problème sir le serveur de destination a besoin du header d'origine. pour le conserver deux options sont utilisable:
```
location / {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;

    # reste de la config
}
```
ici, 'host' se fait attribuer la variable `$host` qui désigne l'host d'origine a l'inverse de `$proxy_host` utilisé par défaut qui utilise le proxy. de la même manière, 'x-real-ip' rassoit l'ip du client faisant la requête.

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
    
    **Résumé** : graph des parte de marché des différent reverse proxy sur le marché
    **Avis sur la ressource** : manque de source mais un tour rapide sur Reddit corrobore la place de nginx comme plus populaire.

[^2]: non mentionné, [guide débutant Nginx](https://nginx.org/en/docs/beginners_guide.html)

    **Résumé** : guide d'installation pour Nginx par Nginx
    **Avis sur la ressource** : manque absolu de détails sur la configuration mais heureusement les autres articles remplisse ce manque.

[^3]: non mentionné, [guide débutant Nginx](https://nginx.org/en/docs/http/ngx_http_proxy_module.html)

    **Résumé** : détail des options de proxy
    **Avis sur la ressource** : très complet mais manque d'exemple

[^4]: non mentionné, [guide débutant Nginx](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)

    **Résumé** : guide de config pour proxy un site web
    **Avis sur la ressource** : très clair et concis