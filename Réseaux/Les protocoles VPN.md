[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# Les protocoles VPN



## Introduction :

Les connexions VPN (Virutal Private Network) permettent de créer un lien sécurisé entre deux points, il existe deux types de connexions VPN : 

1-	Site to Site VPN => entre deux réseaux d’entreprise

![image](https://user-images.githubusercontent.com/71373221/170224719-ced5e30a-1cfd-4f00-965d-46d7571269c3.png)





2-	Client to Site VPN => entre un équipement client et un réseau d’entreprise.

![image](https://user-images.githubusercontent.com/71373221/170224768-f54321ea-6297-4615-ac02-bba596431bf8.png)

 



Le lien créé est appelé un Tunnel VPN, les données qui sont envoyés à travers ce lien sont chiffrés.
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




### L2TP/ Ipsec

L2TP ou Layer 2 Tunneling Protocol fusionne les meilleures fonctionnalités des deux protocoles de tunneling PPTP (Point-to-Point Tunneling Protocol) et L2F (Layer 2 Forwarding Protocol). Il est compatible avec plusieurs plateformes.
Il encapsule les données deux fois, ce qui peut ralentir la connexion. Cependant, il compense cela en fournissant le processus de cryptage/décryptage dans le noyau et en permettant le multithreading.

•	Très sécurisée - fonctionne avec les algorithmes de cryptage AES et 3DES (clé 256 bits)

•	Prend en charge une large gamme de systèmes d'exploitation et mobiles

•	L2TP utilise IPSec pour une sécurité supplémentaire

•	Établissement de liaison fiable - utilise le port UDP 1701, le port 500 et le port 4500

•	L2TP est une bonne option si la sécurité est plus importante pour la connexion VPN que la vitesse.

•	Très facile à configurer




### IKEv2/IPsec : Internet Key Exchange version 2 with IPsec 

IKEv2 est un protocole de tunnellisation. Il est associé à IPsec pour assurer la sécurité du trafic internet. Il est plus rapide que les autres protocoles grâce à la prise en charge de MOBIIKE.
•	Très sécurisée - Prend en charge plusieurs versions d’AES.

•	IKEv2 est pris en charge par moins de systèmes et de logiciels que L2TP/IPSec


•	IKEv2 utilise le port 500 du protocole UDP, donc peut être bloqué par un pare-feu.

•	Connexion stable et cohérente



### OpenVPN

C’est le protocole VPN le plus utilisé, il est compatible avec plusieurs équipements. Il offre une forte sécurité et ne ralentit pas la connexion internet, il est open source donc fiable.
•	Bien adapté aux appareils mobiles et toutes les plateformes.

•	Très sécurisée - Il crypte les données avec un chiffrement complexe, notamment AES et Camellia, des algorithmes de chiffrement 256 bits.


•	Il s’appuie sur SSL/TLS pour l’authentification et le cryptage.

•	Utilisation du chiffrement AES-256 et n’importe quel port. 


•	Open source, ce qui le rend plus fiable

•	Possibilité d’améliorer la Vitesse en utilisant UDP.


•	La configuration est complexe. 




### WireGuard

C’est un protocole récent, développé en 2017 pour Linux, son principal avantage est sa vitesse ultra-rapide.

•	Il est compatible avec windows, macOS, iOS  et Android. 

•	Il utilise deux  clefs une publique et une privée pour gérer l’authentification du serveur et des différents clients. 

•	Utilise le protocole UDP, donc peut être bloqué par un pare-feu.

•	Simple et facile à configurer

•	Utilise ChaCha20 pour le chiffrement et Poly1305 pour l'authentification. Il utilise aussi Curve25519 pour l'échange de clé Diffie-Hellman à courbe elliptique (ECDH) ; BLAKE2s pour le hachage; et un handshake 1.5 Round Trip Time (1.5-RTT) 




## Conclusion

![image](https://user-images.githubusercontent.com/71373221/170264587-6f1faf76-f84a-4282-a497-f8bf6d074df9.png)

Le PPTP est un ancien protocole vulnérable actuellement, il est conseillé de ne plus l’utiliser. 

L2TP est une bonne option si la sécurité est plus importante pour la connexion VPN que la vitesse. IKEv2/IPsec  est le plus rapide, plus que L2TP et PPTP, Il est plus léger et plus stable qu'OpenVPN. Il offre le même niveau de sécurité que L2TP et OpenVPN. 

WireGuard peut être bloqué par les pare-feux car il utilise UDP, comme IKEv2. Il est toujours en cours de développement et doit encore faire l'objet d’audits de sécurité.

OpenVPN est cependant moins susceptible d'être bloqué par les pare-feux lorsque vous vous connectez via TCP. Il est le protocole le plus fiable vue qu’il est opensource.





## Bibliographie


* IPSec Overview Part Four: Internet Key Exchange (IKE)
https://www.ciscopress.com/articles/article.asp?p=25474&seqNum=7

Consulté le 21 mai 2022

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


   
   

