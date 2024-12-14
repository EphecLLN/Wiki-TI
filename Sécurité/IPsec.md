---
layout: default
title: IPSec
parent : Sécurité
---
# IPSec ( Internet Protocol Security )

## Un peu d'histoire

En informatique et plus particulièrement en réseau, les paquets IP sont les éléments principaux pour communiquer des données entre deux machines.
Dans ces paquets on y distingue deux grands types d'éléments à savoir, ceux pour les données et ceux contenant l’adresse de l’expéditeur et du destinataire.

<img width="408" alt="Screenshot 2022-06-02 at 14 59 04" src="https://user-images.githubusercontent.com/43784062/171634672-39e7557e-e713-429f-997d-805fc5f0283e.png">

Le point négatif de cette communication est l’absence de sécurité en effet les données des paquets ne sont pas cryptées de plus il n’est pas possible de vérifier l’authenticité de celles-ci également.

C’est un gros problème vu que lors de cet échange de données, les paquets passent par différents routeurs. Elles peuvent donc être interceptées, lues, manipulées, modifiées à tout moment par une personne mal intentionnée.

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

Le protocole d'en-tête d'authentification (AH) fournit l'authentification de l'origine des données ainsi que l'intégrité de celles-ci[3] . Néanmoins, AH n'assure pas la confidentialité des données, celles-ci sont transmisses en claire !
c'est pourquoi AH est utilisé en combinaison avec ESP.
Le protocoles ESP ou Encapsulating Security Payload est utilisé pour la confidentialité des données en les chiffrants.

La combinaison de ces protocoles possède deux mode de fonctionnnemnt, le mode `Transport` où les machines sont connectées directement et le mode `Tunnel` qui permet de crée une connexion sécurisée entre deux réseaux IP.

- Fonctionnement en mode Transport :

Le mode transport a possède un temps de traitement rapide, mais ne sécurise que les données de l'utilisateur, les adresses source et cible restent non protégées c'est pourquoi ce mode ce mode ne peut être utilisé qu’entre deux machines, par exemple routeur à routeur ou ordinateur à ordinateur.

- Il commence par crypter les données
- Ensuite il rajoute un entête intermédiaire entre l’entête du paquet et les données.
  `<img width="392" alt="Screenshot 2022-06-14 at 10 32 06" src="https://user-images.githubusercontent.com/43784062/173571711-bfeb34d1-9654-4438-9ab8-ecad25ffd460.png">`
- Fonctionnement en mode Tunnel :

 Le paquet recoit une nouvelle en-tête IP contenant les adresse source et cible ainsi que les données.
 Ce mode permet de connectées deux réseaux entre eux.

- Il commence par crypter le paquet contenant les données
- Ensuite il forme un nouveau paquet avec une nouvelle entête IP pour assurer l’intégrité de celui-ci et donc empêcher un pirate de connaitre
  l’expéditeur et le destinataire.

<img width="405" alt="Screenshot 2022-06-14 at 10 31 58" src="https://user-images.githubusercontent.com/43784062/173571730-041d101b-faf3-4bef-9820-7c5e0a042f79.png">

### Gestion des clés IKE & ISAKMP

IKE ou Internet Key Exchange est un procédé de gestion de clés permettant d'assurer la négociation entre deux entités afin de mettre en place un canal de communication sécurisé entre celles-ci. Ce dernier utilise l'algorithme Diffie-Hellaman pour l'échange des clés tout en combinaison avec ISAKMP .

ISAKMP ou Internet Security Association and Key Management Protocol est un protocole défini par RFC 2408 pour établir une association de sécurité (SA) et des clés cryptographiques dans un environnement Internet.[6]

- 1. On établit une connexion sécurisée en utilisant, soit les certificats de chaque partie, soit un mot de passe commun
- 2. Une fois les routeurs d’accord sur le type de sécurité, IKE ouvre un tunnel sécurisé

![IKE](https://user-images.githubusercontent.com/43784062/170671902-771c40ec-7197-4f2e-8774-54dab0226f26.jpeg)

### Base de données SAD & SDP

Les informations nécessaires au transfert des paquets sont stockées dans deux bases de données en local à savoir SPD ( Security Policy Database ) et SAD ( Security Association Database ).

Dans la base de données SDP on va retrouver les information permettant de déterminer le ou les protocoles à utiliser à savoir AH, ESP ou la combinaison des deux tant dit que dans la base de donnée SAD, on va retrouver les informations essentielles pour le protocole IKE à savoir les différentes clés de chiffrement.

## Les points positifs et négatif d'IPSec

Les avantages d’IPSec sont indéniables en matière de performance et de fiabilité.
Une fois le tunnel ouvert, les différentes formes de paquets de données (mail, ftp, voip,... ) peuvent être communiquées sans que des outils/applications n’aient à être installés.

IPSec fourni également une grande sécurité pour le trafic de données interne des entreprises mais cela peut aussi réduire la vitesse de communication.
Deplus IPSec complique la traversée des NAT et Pare-Feu à cause de l’absence de notion de ports source/destination pou résoudre ceci, IPSec utilise une extension "Nat-Traversal, RFC 3947 et RFC 3948" [10 - 11] cette dernière propose d'encapsuler le protocoles ESP ou AH dans un paquet UDP afin de pouvoir plus facilement traverser NAT et Par-Feu.[5 - 7 - 8 - 9]

## Exemple utilisation d'IPSec

Voici un exemple d'utilisation IPSec en mode tunnel :

Prennons par exemple une entreprise qui possède deux sites, celle-ci aimerait que ses deux sites puissent communiquer sur le même réseau de mainère sécurisée. Pour répondre à ce besoin elle va mettre en place un VPN pour relier les deux sites et utiliser la suite de protocoles IPSec afin de créer un tunnel sécursié entre les deux sites.

![Screenshot 2022-06-08 at 15 47 43](https://user-images.githubusercontent.com/43784062/172632849-84ae322a-aad7-4681-88c0-fdca4b8bc8fc.png)

Voici un exemple d'utilisation IPSec en mode transport :

Prennons par exemple, une connexion bureau à distance sécurisée où une personne sur une machine A se connecte sur une seconde machine B ( ex : Teamviewer ). Pour établir une communication sécurisé entre sa machine et la seconde elle va devoir utiliser le mode Transport d'IPSec.

![Screenshot 2022-06-08 at 16 19 19](https://user-images.githubusercontent.com/43784062/172640127-7c37ab62-fd3b-436b-b80e-e88c07368373.png)

## Sources :

[1] https://www.ionos.com/digitalguide/server/know-how/ipsec-security-architecture-for-ipv4-and-ipv6/, consulté le 26/05/2022, date article : 03/08/2016, auteur : /, Affiliation : ionos.com, titre artcile : Secure network connections with IPsec

[2] https://www.frameip.com/vpn/, consulté le 26/05/2022, date article : /, auteur : /, Affiliation : frameip.com, titre artcile : RÉSEAU PRIVÉ VIRTUEL VPN

[3] https://www.ibm.com/docs/en/i/7.1?topic=protocols-authentication-header, consulté le 28/05/2022, date article : 31/08/2021, auteur : / , Affiliation : ibm.com , titre article : Authentication Header

[4] https://en.wikipedia.org/wiki/Internet_Security_Association_and_Key_Management_Protocol, consulté le 28/05/2022, date article : 25/05/2022, auteur : /, Affiliation : wikipedia.org, titre article : Association de sécurité Internet et protocole de gestion de clés

[5] https://www.sstic.org/media/SSTIC2006/SSTIC-actes/Faiblesses_d_IPSec_en_deploiements_reels/SSTIC2006-Article-Faiblesses_d_IPSec_en_deploiements_reels-vanhullebus.pdf, consulté le 31/05/2022, auteusr :Yvan Vanhullebus , Affiliation : Netasq, titre article : Faiblesses d’IPSec en déploiements réels

[6] https://www.rfc-editor.org/rfc/rfc2408.html, consulté le 31/05/2022 , date article : Novembre 1998 ,auteurs : D. Maughan &  M. Schertler &  M. Schneider & J. Turner , Affiliation : RFC, titre article :  Internet Security Association and Key Management Protocol (ISAKMP)

[7] https://www.rfc-editor.org/rfc/rfc3947.html, consulté le 31/05/2022, date article :  Janvier 2005, auteurs :T. Kivinen &  B. Swander & A. Huttunen &  V. Volpe , Affiliation : RFC, titre article :  Negotiation of NAT-Traversal in the IKE

[8] https://www.rfc-editor.org/rfc/rfc2406.html, consulté le 31/05/2022, date article : Novembre 1998 , auteurs :S. Kent &  R. Atkinson , Affiliation : RFC, titre article :  IP Encapsulating Security Payload (ESP)

[9] https://www.rfc-editor.org/rfc/rfc2402.html, consulté le 31/05/2022, date article : Novembre 1998 , auteurs :S. Kent &  R. Atkinson, Affiliation : RFC, titre article : IP Authentication Header

[10] https://www.rfc-editor.org/rfc/rfc3947.html, consulté le 31/05/2022, date article : Janvier 2005 , auteurs : T. Kivinen & B. Swander &   A. Huttunen & V. Volpe , Affiliation : RFC, titre article : Negotiation of NAT-Traversal in the IKE

[11] https://www.rfc-editor.org/rfc/rfc3948.html, consulté le 31/05/2022, date article : Janvier 2005 , auteurs :A. Huttunen &  B. Swander &  V. Volpe & L. DiBurro & M. Stenberg, Affiliation : RFC, titre article : UDP Encapsulation of IPsec ESP Packets
