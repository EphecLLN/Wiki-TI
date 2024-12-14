---
layout: default
title: Quic et TCP
parent: Réseaux
---
# Quic et TCP

## C’est quoi TCP?

TCP est l'abréviation de Transmission Control Protocol. Il s'agit de l'un des protocoles principale de la famille de protocoles TCP/IP et l'une des normes les plus importantes d'Internet. Il s'agit d'un protocole orienté connexion qui utilise les services du protocole Internet au niveau de la commutation.

## C’est quoi Quic? [1]

C’est rare qu’un protocolle basant sur IP apparait. Depuis le 27 mai 2021 la version 1.0 à été publié officielment. QUIC ("Quick UDP Internet Connections") est un protocole de transport expérimental de Google, qui a été mis à la disposition du public pour la première fois en 2013. Afin d'envoyer rapidement et facilement des paquets de données simples via le protocole UDP (User Datagram Protocol) sans connexion. Le travail de QUIC a été motivé par le désir de développer des alternatives aux solutions de sécurité établies pour TCP, HTTP/2 et TLS/SSL. Cela offre la même protection tout en réduisant les retards de connexion et de transport et en permettant plusieurs connexions.

## Mais pourquoi inventer un nouveau protocolle si TCP existe déjà ? [2]

TCP, est un protocole classique permettant d'envoyer des données binaires sans erreur. TCP garantit que tous les paquets sont envoyés sans modification, dans le bon ordre et sans perte. Si nécessaire, la pile TCP réclame les paquets manquants jusqu'à ce que les données soient disponibles. Pour que cela fonctionne, il y a un handshake initial au cours duquel la destination se met d'accord sur différents paramètres. Cela cause qu’au début d’une connection TCP beaucoup de temps est perdu à cause du TCP-Handshake et du TLC-Handshake.
D'autre part, Quic est moins sujet à la perte de paquets et peut donc mieux fonctionner sur des réseaux peu fiables. Quic est un protocole basé sur UDP, spécialement conçu pour être utilisé dans des réseaux rapides, fiables et sécurisés. Il offre un certain nombre d'avantages par rapport à TCP, notamment de meilleures performances et une plus grande fiabilité.
Quic est également très évolutif et peut être facilement adapté aux besoins d'un réseau. Il est en outre très efficace sur le plan énergétique et peut donc être utilisé dans des appareils mobiles, où les fonctions d'économie d'énergie sont avantageuses.

## Quic avec HTTP/3 [3]

Les deux protocoles HTTP3 et QUIC sont les dernières avancées dans le domaine des technologies Internet.

HTTP3 est une évolution du célèbre protocole de transfert hypertexte (HTTP). Alors que HTTP était jusqu'à présent basé sur le protocole TCP/IP, HTTP3 s'appuie sur le nouveau protocole UDP, spécialement conçu pour la transmission de données sur Internet. L'utilisation du protocole UDP permet à HTTP3 de fonctionner beaucoup plus rapidement que le HTTP traditionnel. En outre, HTTP3 offre quelques nouvelles fonctions particulièrement utiles pour l'utilisation d'Internet. Par exemple, plusieurs requêtes peuvent être envoyées simultanément au serveur, ce qui augmente considérablement la vitesse de la connexion. La sécurité est également améliorée par le nouveau protocole, puisque toutes les données sont transmises de manière cryptée.

On ignore encore totalement quand QUIC ou HTTP/3 seront présentés comme norme. En conséquence, les développeurs et les administrateurs ont encore le temps de planifier l'avenir et de paniquer. Il n'est pas vraiment nécessaire de paniquer.  QUIC est facile à mettre en œuvre. Comme l'infrastructure utilisée est le très répandu UDP, il suffit de mettre à jour le logiciel du navigateur sans devoir attendre la prochaine version du système d'exploitation. En outre, QUIC est qualifié de rétrocompatible. Si l'interlocuteur n'est pas encore prêt pour le nouveau protocole, il met à disposition un fallback sur TCP.

## Différence entre Quic et TCP [2] [3]

La plus grande différence est sûrement que Quic ce situe dans le « user space » par rapport à TCP qui ce trouve dans le kernel :
![grafik](https://user-images.githubusercontent.com/56824199/174491370-c7f36cac-c627-4e70-83a4-80d219ff3809.png)

L'avantage de l'implémentation de QUIC dans l'espace utilisateur est que les développeurs peuvent écrire / sélectionner leur propre implémentation et que les développeurs peuvent rapidement diffuser les corrections et améliorations de la pile QUIC via des mises à jour régulières de l'application (comme du côté serveur). La plupart des entreprises utilisent également la pile QUIC uniquement pour communiquer avec leurs services, ce qui est donc également possible dans la plupart des cas. Dans ce cas, l'entreprise possède à la fois les implémentations client et serveur.

## Perfomance Quic vs TCP [4]

![grafik](https://user-images.githubusercontent.com/56824199/174491383-a2e94588-8602-45ba-8a2b-b0bab52e645d.png)

## Bibliographie

[1] Wiki Ionos, „QUIC: Das steckt hinter dem experimentellen Google-Protokoll“,
« 20.07.2020 », https://www.ionos.de/digitalguide/hosting/hosting-technik/quic-das-internet-transportprotokoll-auf-udp-basis/ [05.06.2022]

[2] Frank Carius, „QUIC - UDP mit TLS statt TCP“, « 2021 », https://www.msxfaq.de/netzwerk/grundlagen/quic_udp_mit_tls_statt_tcp.htm [05.06.2022]

[3] Nicky Reinert, „HTTP/3 und QUIC: Was steckt hinter dem nächsten großen Update für HTTP?“, « 08.08.2019 », https://entwickler.de/webentwicklung/http3-und-quic-was-steckt-hinter-dem-nachsten-grossen-update-fur-http/ [05.06.2022]

[4] Frederic Lardinois, „Google Wants To Speed Up The Web With Its QUIC Protocol“, « 18.04.2015 », https://techcrunch.com/2015/04/18/google-wants-to-speed-up-the-web-with-its-quic-protocol/?guccounter=1&guce_referrer=aHR0cHM6Ly9sZXZlbHVwLmdpdGNvbm5lY3RlZC5jb20vd2lsbC1nb29nbGVzLXF1aWMtcHJvdG9jb2wtcmVwbGFjZS10Y3AtNmVkOTkxYTBjYTFl&guce_referrer_sig=AQAAAMih_sDttxvv2RkTzk0RRJOclfaZ4vxcSO-lurMsnuBkQ--QgYwfbsqTjkYe0Xn0nSVsqplskoMP77pnlIUBcBpwmy9cGfpoPW2UgSTqBhs6alxSgdCfAYrz5L2v7UVJHYNagfJBxr5yySWOVZmm_fu8wROwNpsaMj3ttQzt9ESn [05.06.2022]
