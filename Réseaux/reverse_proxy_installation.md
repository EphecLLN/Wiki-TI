---
layout: default
title: Installation Reverse Proxy
parent: Réseaux
---
# installation d'un reverse proxy [^1]

il y a beaucoup de proxy possibles. pour ce tutoriel nous allons nous concentrer sur Nginx. puisque c'est l'un des plus utilisé et qu'il a un grande versatilitée. Pour la suite, nous supposeront également l'utilisation de Linux. Pour Windows voyez [ici](https://nginx.org/en/docs/windows.html)

## 1. intaller Nginx [^2]

nginx supporte un grand nombre de distros [ici](https://nginx.org/en/linux_packages.html)
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

une option commence par un nom, suivit de paramètres séparés par des espaces et finit par un point-virgule
exemple: `error_log logs/error.log error;` place toute les erreurs dans le fichier de log mentionné.

nginx peut répondre des pages statiques sans recourir a une redirection. voici ce que contien la config:
```
http {
    server {
       location / {
          root /data/www;
       }
    }
}
```

Les pages statique c'est très sympa mais si on utilise Nginx c'est pour le proxy. Voici comment ca marche

#### le web [^4]
ceci redirigera tout le trafic vers la nouvelle url `http://www.example.com/link/`
```
http {
    server {
        location / {
            proxy_pass http://www.example.com/link/;
        }
    }
}
```

l'utilisation d'ip est également possible comme ceci
```
location / {
    proxy_pass http://127.0.0.1:8000;
}
```
il est également important de conserver le header et l'origine de la requête en rajoutant ces ligne dans `location`
```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
```

voici donc votre configuration
```
http {
    server {
        location / {
            proxy_set_header Host $host; #conserve le sender
            proxy_set_header X-Real-IP $remote_addr; #conserve le header
            proxy_pass http://127.0.0.1:8000; #adresse de redirection
        }
    }
}
```

#### le mail [^5]

ici avant de mette les serveur en place il faut d'abbord mettre quelques options en place
```
mail {
    server_name mail.example.com; #le domaine utilisé par le serveur mail
    auth_http localhost:9000/cgi-bin/nginxauth.cgi; #le serveur d'autentification (optionel)
    proxy_pass_error_message on; #permet de renvoyer les erreurs vers le client
}
```
maintenant on rajoute les serveurs, un par protocole
```
server {
    listen    25;
    protocol  smtp;
    smtp_auth login plain cram-md5;
} 

server {
    listen    110;
    protocol  pop3;
    pop3_auth plain apop cram-md5;
}

server {
    listen   143;
    protocol imap;
}
```
comme precédamment on précise la destination du proxy avec `proxy_pass 127.0.0.1:8000;`

ce qui donne un config complète comme ceci:
```
mail {
    server_name mail.example.com;
    proxy_pass_error_message on;
    server {
        listen     25;

        protocol   smtp;
        proxy_pass smtp://192.168.1.100:25;
    }

    server {
        listen     143;

        protocol   imap;
        proxy_pass imap://192.168.1.100:143;
    }

    server {
        listen     110;

        protocol   pop3;
        proxy_pass pop3://192.168.1.100:110;
    }
}
```

## Bibliographie

[^1]: non mentionné, [RFC 1034 - DOMAIN NAMES - CONCEPTS AND FACILITIES](https://www.wappalyzer.com/technologies/reverse-proxies/#:~:text=Reverse%20proxies%20technologies%20market%20share%20These%20are%20the,in%202023.%20Nginx%2094.6%25%20Envoy%205%25%20Other%200.4%25), 2023
    
    **Résumé** : graph des parte de marché des différent reverse proxy sur le marché
    **Avis sur la ressource** : manque de source mais un tour rapide sur reddit corobore la place de nginx comme plus populaire.

[^2]: non mentionné, [guide débutant Nginx](https://nginx.org/en/docs/beginners_guide.html)

    **Résumé** : guide d'installation pour Nginx par Nginx
    **Avis sur la ressource** : manque absolu de détails sur la configuration mais aureusement les autres articles remplisse ce manque.

[^3]: non mentionné, [guide débutant Nginx](https://nginx.org/en/docs/http/ngx_http_proxy_module.html)

    **Résumé** : détail des options de proxy
    **Avis sur la ressource** : très complet mais manque d'exemple

[^4]: non mentionné, [guide débutant Nginx](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)

    **Résumé** : guide de config pour proxy un site web
    **Avis sur la ressource** : très clair et conci

[^5]: non mentionné, [guide débutant Nginx](https://docs.nginx.com/nginx/admin-guide/mail-proxy/mail-proxy/)

    **Résumé** : guide de config pour proxy un serveur mail
    **Avis sur la ressource** : très clair et conci