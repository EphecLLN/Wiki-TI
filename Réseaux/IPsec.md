---
layout: default
title: LDAP
parent : Réseaux
---

# IPsec ( Internet Protocol Security )

## Un peu d'histoire

En informatique et plus particulièrement en réseau, les paquets sont les éléments principaux pour communiquer des données. On y distingue deux grands types de paquets, ceux pour les données et ceux contenant l’adresse de l’expéditeur et du destinataire.

Le point négatif de cette communication est l’absence de sécurité en effet les données ne sont pas cryptées de plus il n’est pas possible de vérifier l’authenticité des paquets également.

C’est un gros problème vu que lors de cet échange de données, les paquets passe par différents routeurs. Elles peuvent donc être interceptées, lues, manipulées, modifiées à tout moment par une personne mal intentionnée.

Pour faire face à cette situation et ainsi permettre un transfert de paquets de données sécurisés sur des réseaux publics, l’Internet Protocol Security ou IPsec a été développé.

## IPsec 

« Internet Protocol Security » (IPSec) est une suite de protocoles conçue pour le protocole Internet TCP-IP standardisé par l’IETF (Internet Engineering Task Force) pour l’IPv4 mais aussi l’IPv6. Permettant une communication sécurisée avec des réseaux IP qui ne sont pas dignes de confiance. La confidentialité, l’authenticité et l’intégrité de la circulation des données sont assurées par un système d'authentification et de cryptage . 

On peut répartir suite des protocles IPSec en 3 groupes : 

  - Protocoles de transfert : 
  
     - AH ( Authentication Header )
     - ESP ( Encapsulating Security Payload ) 
    
  - Gestion des clés :
  
     - ISAKMP ( Internet Security Association and Key Management Protocol )
     - IKE ( Internet Key Exchange )
  
  - Base de données :
 
    - SAD ( Security Association Database )
    - SPD ( Security Policy Database )

### Protocoles de transfert ESP

ESP ou Encapsulating Security Payload est utilisé pour la sécurité et l’intégrité des données. Il assure l’intégrité des données et l'authentification de la source mais aussi crypte les données. ESP possède deux mode de fonctionnnemnt, le mode `Transport` où machines sont connectées directement et le mode `Tunel` qui permet de crée une connexion sécurisée entre deux réseaux IP.

- Fonctionnement en mode Transport :

Ce mode ne peut être utiliser qu’entre deux machines. 
![Mode transport-1](https://user-images.githubusercontent.com/43784062/170652551-5507034c-ea74-4219-bbcf-73dcdcd0af5f.jpg)


  - Il commence par crypter les données
  - Ensuite il rajoute un entête intermédiaire sans modifier l’entête du paquet

![mode transport](https://user-images.githubusercontent.com/43784062/170522344-5ca1b9d9-67f9-483d-a710-d765e58d9b34.jpg)


- Fonctionnement en mode Tunnel :
 
 Ce mode permet de connectées d"uc réseaux entre eux.
 
 ![Mode tunel-1](https://user-images.githubusercontent.com/43784062/170652643-d34f760a-1580-47cc-bca2-3de177f9d8bf.jpg)


  - Il commence par crypter le paquet contenant les données
  - Ensuite il forme un nouveau paquet avec une nouvelle entête IP pour assurer l’intégrité de celui-ci et donc empêcher un pirate de connaitre
l’expéditeur et le destinataire.


![mode tunel](https://user-images.githubusercontent.com/43784062/170522375-ec2addef-bcf3-47ca-8e9e-dbe29e728458.jpg)


### IKE

IKE ou Internet Key Exchange est un procédé de gestion de clé utilisé pour gérer la connexion entre deux routeurs.
