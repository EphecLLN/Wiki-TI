---
layout: default
title: IPSec
parent : Réseaux
---

# IPSec ( Internet Protocol Security )

## Un peu d'histoire

En informatique et plus particulièrement en réseau, les paquets sont les éléments principaux pour communiquer des données. On y distingue deux grands types de paquets, ceux pour les données et ceux contenant l’adresse de l’expéditeur et du destinataire.

Le point négatif de cette communication est l’absence de sécurité en effet les données ne sont pas cryptées de plus il n’est pas possible de vérifier l’authenticité des paquets également.

C’est un gros problème vu que lors de cet échange de données, les paquets passe par différents routeurs. Elles peuvent donc être interceptées, lues, manipulées, modifiées à tout moment par une personne mal intentionnée.

Pour faire face à cette situation et ainsi permettre un transfert de paquets de données sécurisés sur des réseaux publics, l’Internet Protocol Security ou IPsec a été développé.

## IPSec 

« Internet Protocol Security » (IPSec) est une suite de protocoles conçue pour la suite de protocoles Internet TCP-IP standardisé par l’IETF (Internet Engineering Task Force) pour l’IPv4 mais aussi l’IPv6 permettant une communication sécurisée avec des réseaux IP qui ne sont pas dignes de confiance. La confidentialité, l’authenticité et l’intégrité de la circulation des données sont assurées par un système d'authentification et de cryptage . 

On peut répartir la suite des protocles IPSec en 3 groupes : 

  - Protocoles de transfert : 
  
     - AH ( Authentication Header )
     - ESP ( Encapsulating Security Payload ) 
    
  - Gestion des clés :
  
     - ISAKMP ( Internet Security Association and Key Management Protocol )
     - IKE ( Internet Key Exchange )
  
  - Base de données :
 
    - SAD ( Security Association Database )
    - SPD ( Security Policy Database )

### Protocoles de transfert AH et ESP

Le protocole d'en-tête d'authentification (AH) fournit l'authentification de l'origine des données ainsi que l'intégrité de celles-ci[3] . Néanmoins, AH n'assure pas la confidentialité des données, celle-ci sont transmisses en claire !
c'est pourquoi AH est utilisé en combinaison avec ESP.
Le protocoles ESP ou Encapsulating Security Payload est utilisé pour la confidentialité des données en les chiffrants.

La combinaison de ces protocoles possède deux mode de fonctionnnemnt, le mode `Transport` où machines sont connectées directement et le mode `Tunel` qui permet de crée une connexion sécurisée entre deux réseaux IP.

- Fonctionnement en mode Transport :

Le mode transport a possède un temps de traitement rapide, mais ne sécurise que les données de l'utilisateur, les adresses source et cible restent non protégées c'est pourquoi ce mode ce mode ne peut être utilisé qu’entre deux machines, par exemple routeur à routeur.

![mode transport](https://user-images.githubusercontent.com/43784062/170671693-7d0c304d-b17b-475f-8b17-69aee9f934fc.jpeg)

  - Il commence par crypter les données
  - Ensuite il rajoute un entête intermédiaire entre l’entête du paquet et les données.

![mode transport-1 (1)](https://user-images.githubusercontent.com/43784062/170671717-796d871d-dcfe-45f1-a1ec-e915cb5f8160.jpg)


- Fonctionnement en mode Tunnel :
 
 Le paquet recoit une nouvelle en-tête IP contenant les adresse source et cible ainsi que les données.
 Ce mode permet de connectées deux réseaux entre eux.
 
![mode tunnel](https://user-images.githubusercontent.com/43784062/170671822-316aeaa2-8785-4c66-943a-b0c8b0f8742d.jpg)


  - Il commence par crypter le paquet contenant les données
  - Ensuite il forme un nouveau paquet avec une nouvelle entête IP pour assurer l’intégrité de celui-ci et donc empêcher un pirate de connaitre
l’expéditeur et le destinataire.

![mode tunnel -1](https://user-images.githubusercontent.com/43784062/170671863-06d7e744-6ceb-4831-9136-4cb51cbb58c2.jpg)


### Gestion des clés IKE & ISAKMP

IKE ou Internet Key Exchange est un procédé de gestion de clé utilisé pour gérer la connexion entre deux routeurs. Ce dernier utilise l'algorithme Diffie-Hellaman pour l'échange des clés tout en combinaison avec ISAKMP .
ISAKMP ou Internet Security Association and Key Management Protocol  est un protocole défini par RFC 2408 pour établir une association de sécurité (SA) et des clés cryptographiques dans un environnement Internet.[6]

- 1. On établit une connexion sécurisée en utilisant, soit les certificats de chaque partie, soit un mot de passe commun
- 2. Une fois les routeurs d’accord sur le type de sécurité, IKE ouvre un tunnel sécurisé

![IKE](https://user-images.githubusercontent.com/43784062/170671902-771c40ec-7197-4f2e-8774-54dab0226f26.jpeg)

### Base de données SAD & SDP

Les informations nécessaires au transfert des paquets sont stockées dans deux bases de données en local à savoir SPD ( Security Policy Database ) et SAD ( Security Association Database ).
Dans la base de donnée SDP on va retrouver les information permettant de déterminer le ou les protocoles à utiliser à savoir AH, ESP ou la combinaison des deux tant dit que dans la base de donnée SAD, on va retrouver les informations essentielles pour le protocole IKE à savoir les différentes clés de chiffrement.

## Les points positifs et négatif d'IPSec

Les avantages d’IPSec sont indéniables en matière de performance et de fiabilité. 
Une fois le tunnel ouvert, les différentes formes de paquets de données (mail, ftp, voip,... ) peuvent être communiquées sans que des outils/applications n’aient à être installés.

IPSec fourni également une grande sécurité pour le trafic de données interne des entreprises mais cela peut aussi réduire la vitesse de communication.
Deplus IPSec complique la traversée des NAT et Pare-Feu à cause de l’absence de notion de ports source/destination pou résoudre ceci, IPSec utilise une extension "Nat-Traversal, RFC 3947 et RFC 3948" cette dernière propose d'encapsuler le protocoles ESP ou AH dans un paquet UDP afin de pouvoir plus facilement traverser NAT et Par-Feu.[5 - 7 - 8]

## Sources :
[1] https://www.ionos.com/digitalguide/server/know-how/ipsec-security-architecture-for-ipv4-and-ipv6/, consulté le 26/05/2022, date article : 03/08/2016, auteur : /, Affiliation : ionos.com, titre artcile : Secure network connections with IPsec

[2] https://www.frameip.com/vpn/, consulté le 26/05/2022, date article : /, auteur : /, Affiliation : frameip.com, titre artcile : RÉSEAU PRIVÉ VIRTUEL VPN

[3] https://www.ibm.com/docs/en/i/7.1?topic=protocols-authentication-header, consulté le 28/05/2022, date article : 31/08/2021, auteur : / , Affiliation : ibm.com , titre article : Authentication Header

[4] https://en.wikipedia.org/wiki/Internet_Security_Association_and_Key_Management_Protocol, consulté le 28/05/2022, date article : 25/05/2022, auteur : /, Affiliation : wikipedia.org, titre article : Association de sécurité Internet et protocole de gestion de clés

[5] https://www.sstic.org/media/SSTIC2006/SSTIC-actes/Faiblesses_d_IPSec_en_deploiements_reels/SSTIC2006-Article-Faiblesses_d_IPSec_en_deploiements_reels-vanhullebus.pdf, consulté le 31/05/2022, auteur :Yvan Vanhullebus , Affiliation : Netasq, titre article : Faiblesses d’IPSec en déploiements réels

[6] https://www.rfc-editor.org/rfc/rfc2408.html

[7] https://www.rfc-editor.org/rfc/rfc3947.html

[8] https://www.rfc-editor.org/rfc/rfc3948.html
