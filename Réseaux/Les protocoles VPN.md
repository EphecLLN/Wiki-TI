[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# Les protocoles VPN



## Introduction :

Les connexions VPN (Virutal Private Network) permettent de créer un lien sécurisé entre deux points, elles sont utilisées par les entreprises pour fournir à leurs employés un moyen d'accéder au réseau interne ou à des ressources internes depuis un ordinateur ou un réseau distant. Les connexions VPN sont chiffrées ce qui rend le trafic opaque pour les personnes non autorisées. Il existe aussi des connexions VPN commerciaux destinés aux internautes pour chiffrer leurs trafic et garantir leurs anonymat ainsi que contourner la censure. Cet article est consacré aux VPN qui passent à travers internet, donc n'abordera pas les VPN privés tel que les VPN MPLS.

La première connexion VPN a été développé par un employé de Microsoft en 1996. Il a développé le “point-to-point tunneling protocol” (PPTP).

Il existe trois types de connexions VPN : 
 

1-	Site to Site VPN => entre deux réseaux d’entreprise

![image](https://user-images.githubusercontent.com/71373221/172702429-810763b1-7064-47d3-82c4-985e9d6fa493.png)





2-	Client to Site VPN => entre un équipement client et un réseau d’entreprise.

![image](https://user-images.githubusercontent.com/71373221/172702490-6885a770-2e6d-4a71-a2a5-700b60072871.png)

 3-	SSL-VPN: Les entreprises utilisent des VPN SSL pour permettre aux utilisateurs distants d'accéder en toute sécurité aux services offert , ainsi que pour sécuriser les sessions Internet des utilisateurs qui accèdent à Internet depuis l'extérieur du réseau de l'entreprise.

![image](https://user-images.githubusercontent.com/71373221/172167250-c4a129a1-8677-4987-a332-770f594a8b81.png)



Le choix du protocole VPN à utiliser se base sur trois facteurs:

1-	Les algorithmes de chiffrement utilisés donc le niveau de sécurité et de cryptage offert

2-	La vitesse de navigation

3-	La facilité de mise en place


## Les protocoles VPN




### PPTP signifie Point-to-Point Protocol

C’est le protocole VPN le plus ancien, ses plus gros avantages sont la vitesse qu’il offre et la facilité de configuration. Son cryptage est faible (128 bits) donc facilement piratable
 
•	Ce protocole est compatible avec toutes les plateformes : Windows, Linux, macOS, iOS, Android, Tomato, DD-WRT et d’autres systèmes d’exploitation et appareils.

•	Pour établir une connexion  PPTP il faut l’adresse du serveur, un nom d’utilisateur et un mot de passe.

•	Utilisation des clés de cryptage jusqu’à 128 bits.

•	Utilise le chiffrement MPPE (Microsoft Point-to-Point Encryption).

PPTP est un protocole de niveau 2, le principe de ce protocole est de crypter et compresser des trames PPP et de les encapsuler dans un datagramme IP. Les données du réseau local sont encapsulées dans un message PPP, qui est lui-même encapsulé dans un message IP.
Le schéma suivant montre comment un paquet PPTP est assemblé.

![image](https://user-images.githubusercontent.com/71373221/172167318-0a443f0d-833c-4158-bf44-e4ae609382c6.png)



### L2TP/ Ipsec

L2TP ou Layer 2 Tunneling Protocol fusionne les meilleures fonctionnalités des deux protocoles de tunneling PPTP (Point-to-Point Tunneling Protocol) et L2F (Layer 2 Forwarding Protocol). Il est compatible avec plusieurs plateformes.
Il encapsule les données deux fois, ce qui peut ralentir la connexion. Cependant, il compense cela en fournissant le processus de cryptage/décryptage dans le noyau et en permettant le multithreading.

•	Très sécurisée - fonctionne avec les algorithmes de cryptage AES et 3DES (clé 256 bits)

•	Prend en charge une large gamme de systèmes d'exploitation et mobiles

•	L2TP utilise IPSec pour une sécurité supplémentaire

•	Établissement de liaison fiable - utilise le port UDP 1701, le port 500 et le port 4500

•	L2TP est une bonne option si la sécurité est plus importante pour la connexion VPN que la vitesse.

•	Très facile à configurer

L2TP est un protocole de niveau 2, le principe de ce protocole est de crypter des trames PPP et de les encapsuler dans un datagramme IP qui est crypté avec IPsec. 
IPsec permet de sécuriser les échanges au niveau de la couche réseau. 
Le schéma suivant montre comment un paquet L2TP/IPsec est assemblé.

![image](https://user-images.githubusercontent.com/71373221/172167379-f74de74f-d68d-41e8-a7b7-a07404487c39.png)

### SSTP (Secure Socket Tunneling Protocol):
  
SSTP est un  protocole VPN développé par Microsoft pour remplacer PPTP et L2TP/IPSec. Il a été introduit pour améliorer la sécurité des transferts de données et éviter les blocages des pare-feu. Il utilise SSL/TLS, des négociations de clés sécurisées et des transferts cryptés.
Lorsqu'un client établit une connexion VPN basée sur SSTP, il établit d'abord une connexion TCP au serveur SSTP via le port TCP 443. L'établissement de liaison SSL/TLS se produit via cette connexion TCP.

•	Disponible sur Linux et Mac OS X mais il est utilisé principalement sur Windows.

•	SSTP utilise le chiffrement AES-256 (Advanced Encryption Standard) 

•	Facile à installer et à configurer

•	Utilise TCP sur le port 443 ce qui le rend difficile à bloquer 

SSTP est un protocole de niveau transport ,  les paquets IP sont chiffrés, encapsulés puis transportés dans un canal SSL. 
Le schéma suivant montre comment un paquet SSTP est assemblé.


![image](https://user-images.githubusercontent.com/71373221/172168007-9ef2bcdf-a25c-4acf-98d2-f666cbda1b03.png)


### IKEv2/IPsec : Internet Key Exchange version 2 with IPsec 

IKEv2 est un protocole de tunnellisation. Il est associé à IPsec pour assurer la sécurité du trafic internet. Il est plus rapide que les autres protocoles grâce à la prise en charge de MOBIIKE.
•	Très sécurisée - Prend en charge plusieurs versions d’AES.

•	IKEv2 est pris en charge par moins de systèmes et de logiciels que L2TP/IPSec


•	IKEv2 utilise le port 500 du protocole UDP, donc peut être bloqué par un pare-feu.

•	Connexion stable et cohérente

IKEv2 est un protocole de niveau application, il permet l’échange de clés secrètes de façon sécurisée. Le principe de ce protocole est de chiffrer des messages en utilisant des clés pré-partagées. IPsec permet de sécuriser les échanges au niveau de la couche réseau. 
IKE combine des éléments issus de protocoles différents :
-	ISAKMP : Internet Security Association and Key Management Protocol (RFC 2408) 
-	OAKLEY : Oakley Key Determination Protocol (RFC 2412) 
-	DOI : IPSec Domain of Interpretation (RFC 2407) 

Le schéma suivant montre l’emplacement de IKE et IPsec dans les couches TCP/IP.

![image](https://user-images.githubusercontent.com/71373221/172167414-8ffaa2f6-dc8e-4c1b-b80c-b58e895b7f19.png)


### OpenVPN

C’est le protocole VPN le plus utilisé, il est compatible avec plusieurs équipements. Il offre une forte sécurité et ne ralentit pas la connexion internet, il est open source donc fiable.
•	Bien adapté aux appareils mobiles et toutes les plateformes.

•	Très sécurisée - Il crypte les données avec un chiffrement complexe, notamment AES et Camellia, des algorithmes de chiffrement 256 bits.


•	Il s’appuie sur SSL/TLS pour l’authentification et le cryptage.

•	Utilisation du chiffrement AES-256 et n’importe quel port. 


•	Open source, ce qui le rend plus fiable

•	Possibilité d’améliorer la Vitesse en utilisant UDP.


•	La configuration est complexe. 

Le transfert de données démarre après l'échange de la clé symétrique en utilisant la connexion TLS déjà établie. OpenVPN utilise un tunnel UDP sans connexion où il n'y a pas de mécanisme de retransmission et ACK. La structure de paquet ainsi que l'encapsulation de la couche réseau peuvent être vues dans l’image ci-dessous.
L'en-tête de paquet se compose uniquement d'opcode et d'ID de clé. L'opcode est utilisé pour identifier le type de paquet, l'ID de clé est utilisé pour identifier l'état TLS local associé. 

![image](https://user-images.githubusercontent.com/71373221/172167457-f08c2733-d506-41a2-b0d6-cdaff62bb6be.png)



### WireGuard

C’est un protocole récent, développé en 2017 pour Linux, son principal avantage est sa vitesse ultra-rapide.

•	Il est compatible avec windows, macOS, iOS  et Android. 

•	Il utilise deux  clefs une publique et une privée pour gérer l’authentification du serveur et des différents clients. 

•	Utilise le protocole UDP, donc peut être bloqué par un pare-feu.

•	Simple et facile à configurer

•	Utilise ChaCha20 pour le chiffrement et Poly1305 pour l'authentification. Il utilise aussi Curve25519 pour l'échange de clé Diffie-Hellman à courbe elliptique (ECDH) ; BLAKE2s pour le hachage; et un handshake 1.5 Round Trip Time (1.5-RTT) 

Le protocole WireGuard fournit un tunnel réseau sécurisé entre deux terminaux en utilisant le protocole UDP (User Datagram Protocol) comme protocole de transport. Il utilise un protocole cryptographique de handshake appelé Noise pour fournir une authentification mutuelle et l’accord sur la clé. Les données de transport telles que les paquets IP encapsulés dans les tunnels WireGuard sont protégées à l'aide du chiffrement authentifié avec données supplémentaires (AEAD).


## Conclusion

![image](https://user-images.githubusercontent.com/71373221/172167603-14fd30d6-0057-47b3-8cc6-dc9a989246bc.png)


Le PPTP est un ancien protocole vulnérable actuellement, il est conseillé de ne plus l’utiliser. 

L2TP est une bonne option si la sécurité est plus importante pour la connexion VPN que la vitesse. IKEv2/IPsec  est le plus rapide, plus que L2TP et PPTP, Il est plus léger et plus stable qu'OpenVPN. Il offre le même niveau de sécurité que L2TP et OpenVPN. 

WireGuard peut être bloqué par les pare-feux car il utilise UDP, comme IKEv2. Il est toujours en cours de développement et doit encore faire l'objet d’audits de sécurité.

OpenVPN est cependant moins susceptible d'être bloqué par les pare-feux lorsque vous vous connectez via TCP. Il est le protocole le plus fiable vue qu’il est opensource.

SSTP est un protocole propriétaire de Microsoft, s’il faut choisir entre PPTP, L2TP et SSTP, pour un ordinateur Windows, il vaut mieux utiliser SSTP car il peut contourner les pares-feux et il est plus sécurisé que PPTP.




## Bibliographie


* IPSec Overview Part Four: Internet Key Exchange (IKE)
https://www.ciscopress.com/articles/article.asp?p=25474&seqNum=7

Consulté le 21 mai 2022
Date de publication : Feb 22, 2002.
L'article est écrit par Cisco Press .


Résumé : Article écrit par Cisco et décrit les protocoles IPSec et IKE1&2. 
Avis sur la resource : Excellent article, il donne des détails technique approfondis sur les protocoles IPSec et IKE1&2.

* Les différents protocoles utilisées par les fournisseurs VPN
https://www.vpnmonde.com/les-differents-protocoles-pour-les-vpn/

Consulté le 20 mai 2022

Résumé : Article écrit par des bénévoles en sécurité informatique, il analyse et compare les différents protocoles VPN du marché. 

Avis sur la resource : Excellent article, il explique en détails les protocoles VPN et donne les avantages et inconvénients de chaque protocole.


* Understanding VPN Protocols: Which One is Best? | CyberNews
https://cybernews.com/what-is-vpn/vpn-protocols/

Consulté le 13 mai 2022

Résumé : Cet article explique les protocoles VPN, il compare aussi les différents protocoles utilisés. 

Avis sur la resource : ce document décrit bien les protocoles VPN et fait une comparaison détaillée. 

* Secure Socket Tunneling Protocol (SSTP).: https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-sstp/70adc1df-c4fe-4b02-8872-f1d8b9ad806a

Consulté le 5 Juin 2022

Date de publication: 06/24/2021.

L'article est écrit par Microsoft.

Résumé : Article écrit par Microsoft et décrit le protocole SSTP. 
Avis sur la ressource : Excellent article, il donne des détails technique approfondis sur le protocole SSTP.

* Les VPNs et les protocoles SLIP, PPP, PPTP, L2F, L2TP, LCP, IPSec, MPLS, NAT: http://wapiti.enic.fr/commun/ens/peda/options/ST/RIO/pub/exposes/exposesrio2001ttv02/Roudel_Maroc/index.htm

Consulté le 6 Juin 2022

Date de publication: Janvier 2002.

L'article est écrit par  Philippe Roudel et Alain Maroc ENIC.

Résumé : Cet article décrit les protocoles suivants: SLIP, PPP, PPTP, L2F, L2TP, LCP, IPSec, MPLS, NAT. 
Avis sur la ressource : Excellent article, il donne des détails technique approfondis sur tous les protocoles abordés.


   
   

