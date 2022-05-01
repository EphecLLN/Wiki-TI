---
layout: default
title: HTTPS
parent: Réseaux
---

[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)

# HTTPS

## Qu'est-ce que HTTPS ?

Le HTTPS est un protocole permettant de sécuriser les échanges de données entre un serveur et un client, et de valider l’identité d’un site visité. Cette double sécurisation est essentielle : elle garantit la confidentialité des données et rassure les internautes qui se connectent à votre site web.

C'est le protocole de transfert HTTP qui est encapsulé au sein d'une session SSL ou TLS.

![image](https://user-images.githubusercontent.com/91373823/166163525-721fb773-5093-4672-8812-efc90ac67c41.png)

###

## Pourquoi HTTPS ?

-   Meilleur référencement et positionnement par l'algorithme de Google.

-   Protéger les informations des internautes et de vos clients en évitant les attaques de l'homme du milieu ( l'interception du trafic).

-   Éviter d’avoir le label “Non sécurisé” à l’affichage de votre site internet.

-   Pour bénéficier de HTTP/2.0 <br> La norme n'impose pas HTTPS mais les navigateurs Firefox et Chrome requiert HTTPS pour se connecter à un site en HTTP/2.0.

## Objectifs d'HTTPS

1. Confidentialité :

    Les données échangées sont chiffrées à l'aide d'une clé et ne peuvent être déchiffrées par les deux parties.

2. Authentification :

    Le client vérifie que le serveur qu'il adresse est celui qu'il prétend être (ceci évite les attaques de l'homme du milieu). Au besoin, le client peut vérifier son identité auprès du serveur.

3. Intégrité :

    Les parties s'assurent que les données reçues n'ont pas fait l'objet d'une modification.

4. Transparence :

    Aucun changement nécessaire dans le protocole applicatif n'englobé.

5. Spontanéité :

    Un client peut spontanément se connecter à un serveur (pas de prérequis).

### Comprendre le SSL/TLS

SSL signifie Secure Sockets Layer, qui est le nom du protocole lorsqu'il appartenait à Netscape.
Mais l'IETF (Internet engineering Task Force) a décidé d'en prendre le contrôle afin de le rendre plus ouvert et standardisé.
Mais pour éviter tous les problèmes légaux, il est renommé TLS pour « Transport Layer Security ».

Aujourd'hui il n'y a plus que TLS mais on fait référence à SSL par abus de langage.

### Versions SSL/TLS

-   SSL 1.0 (jamais publié) par Netscape,
-   SSL 2.0 en 1995, déprécié en 2011 par la RFC 6176,
-   SSL 3.0/RFC 6101 en 1996 (refonte), déprécié en 2015 par la RFC 7568,
-   TLS 1.0 / RFC 2246 en 1999 (modifications mineures),
-   TLS 1.1 / RFC 4346 en 2006 (protections contre les attaques sur CBC),
-   TLS 1.2/ RFC 5246 en 2008 (chiffrement authentifié ou AEAD via GCM et CCM),
-   Mise à jour sur TLS 1.2 en 2011/ RFC 6176 (compatibilité SSL),
-   TLS 1.3 / RFC 8446 en 2018 (compression, renégociation, MD5, SHA1, RC4, 3DES, PFS obligatoire, optimisation réseau (0-RTT) , SHA-256, Courbe elliptique 25519 & 448).

### Rappels cryptographiques

Le chiffrement symétrique et asymétrique sont les deux techniques utilisées pour protéger la confidentialité de votre message sur internet. La différence fondamentale entre les deux réside dans le fait que le chiffrement symétrique permet de chiffrer et de déchiffrer le message à l'aide de la même clé. En revanche, le chiffrement asymétrique utilise la clé publique pour le chiffrement et une clé privée pour le déchiffrage.

|             |                 Chiffrement symétrique                 |                   Chiffrement asymétrique                   |
| :---------: | :----------------------------------------------------: | :---------------------------------------------------------: |
| Définition  | Une seule clé pour le chiffrement et le déchiffrement. | Une clé différente pour le chiffrement et le déchiffrement. |
| Performance |                  Rapide en exécution.                  | Lent à l’exécution en raison de la charge de calcul élevée. |
| Algorithmes |                 AES, DES, 3DES et RC4.                 |                    Diffie-Hellman, RSA.                     |
|  Objectif   |   Utilisé pour la transmission massive des données.    |      Souvent utilisé pour l'échange des clés secrètes.      |

## Authentification

Un certificat c'est comme la carte d'identité d'un serveur, il permet d'assurer que vos etes bien la personne qui prétender l'être. Pour cela, il faudrait passer par une autorité de certificat (autorité reconnu, validé, sure et peut délivrer des certificats). Il faudrait donc prouver que nous sommes le propriétaire du site à l'autorité de certification et, si tel est le cas, nous recevons ce certificat.

Il existe 3 types de certificats.

1. <b> Validation de domaine </b><br>
   Simple vérification sur l’appartenance du domaine.
2. <b> Validation d’organisation </b><br>
   Vérification de l’identité du propriétaire (carte d’identité, SIRET, etc).
3. <b> Validation étendue </b><br>
   Enquête sur le propriétaire (vérification d’une présence physique, opérationnelle et juridique), permet l’affichage du propriétaire dans la barre d’adresse).

### Let's Encrypt

L'idée est d'installer un programme et que notre site passe en HTTPS sans rien faire et il est GRATUIT.

C'est un ensemble de choses, tous d'abord c'est une autorité de certification qui est bien connue par les navigateurs. C'est également un protocole qui s'appelle ACME et enfin c'est un agent Open Source qui va négocier avec les serveurs de Le't Encrypt pour prouver que vous êtes bien le propriétaire du site afin de nous installer un certificat de type Domaine Validation et il va le maintenir toujours à jour.

## Mise en pratique

Si vous avez un site HTTP en production sous apache et que vous souhaiatez le mettre en HTTPS, la solution la plus simple consiste à utiliser l'outil [Certbot](https://doc.ubuntu-fr.org/apache2#activation_du_module_ssl) de [Let's Encrypt](https://letsencrypt.org/fr/).

Voici les étapes à suivre :

Pour que le protocole TLS fonctionne sous Apache2, vous devez activer le module ssl à l'aide de la commande :

```
sudo a2enmod ssl
```

puis recharger la configuration d'Apache 2 :

```
sudo systemctl reload apache2
```

Mise en place de HTTPS avec Certbot, pour celà il faut que votre service web Apache soit déjà configuré, fonctionnel et accessible au public.

Installation de Certbot

```
sudo apt install python3-certbot-apache
```

Certbot permet ensuite de générer tous les certificats et d'adapter les configurations d'Apache pour tous les noms de domaine associés aux hôtes virtuels au moyen d'une seule commande :

```
sudo certbot --apache
```

À l'aide de l'option `–apache`, Certbot crée et active automatiquement les fichiers de configuration pour les serveurs virtuels HTTPS sur le port 443.

Au cours de l'opération, le script nous demande de vérifier les domaines pour lesquels nous voulons obtenir les certificats et de choisir d'utiliser HTTPS ou non. Acceptés tous et forcer le HTTPS.

Après cette opération les sites devraient être accessibles en HTTPS de manière sécurisée.

Vous trouverez une documentation plus détaillée à ce sujet sur [cette page](https://doc.ubuntu-fr.org/tutoriel/securiser_apache2_avec_ssl) de la documentation.

Voilà, nous avons terminé ! Nous avons appris ce qu'est le HTTPS et comment le mettre en place.

## Bibliographie

-   [Qu’est ce que SSL/TLS et HTTPS ?](https://www.hostinger.fr/tutoriels/quest-ce-que-ssl-tls-et-https), Bernadeta Kairyte, 08 mars 2022, consulté le 1er mai 2022.

    -   Résumé : Présentation des protocoles de chiffrement et de sécurité.
    -   Avis sur la ressource : Très bon résumé concernant le protocole SSL/TLS et HTTPS.

-   [Différence entre le cryptage symétrique et asymétrique](https://waytolearnx.com/2018/07/difference-entre-le-cryptage-symetrique-et-asymetrique.html#comment-2889), Anonyme, 23 juillet 2022, consulté le 30 avril 2022

    -   Résumé : Explication de la différence entre le cryptage symétrique et asymétrique.
    -   Avis sur la ressource : Resumé assez court mais très pertinent.

-   [Types de certificats SSL : Lequel convient le mieux à votre site ?](https://kinsta.com/fr/blog/types-de-certificats-ssl/), Salman Ravoof, 28 juillet 2021, consulté le 01 mai 2022

    -   Résumé : Les différents types de certificats SSL.
    -   Avis sur la ressource : Explication dans les grandes lignes.

-   [SSL/TLS and PKI History](https://www.feistyduck.com/ssl-tls-and-pki-history/), Ivan Ristić, février 2022, consulté le 27 avril 2022

    -   Résumé : L'histoire du protocole SSL/TLS et de la PKI.
    -   Avis sur la ressource : Résume assez long mais très pertinent.

-   [Utiliser HTTPS avec Apache2](https://doc.ubuntu-fr.org/tutoriel/securiser_apache2_avec_ssl), Gabriel Theuws, 16 mars 2021, consulté le 25 avril 2022

    -   Résumé : Mise en place de certbot dans un ser apache.
    -   Avis sur la ressource : Les etapes de configuration sont très bien expliquées.

-   [Mettre son site en ligne (4/4) : Comprends le SSL / HTTPS](https://www.youtube.com/watch?v=_UpuZ0Y3k-c), Jonathan Boyer, 24 février 2016, consulté le 26 avril 2022

    -   Résumé : Vidéo expliquant le fonctionnement des protocoles SSL/TLS et HTTPS.
    -   Avis sur la ressource : Video de qualité et les démonstantions sont bien illustrées.
