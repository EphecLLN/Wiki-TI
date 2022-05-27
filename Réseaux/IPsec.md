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

![mode transport](https://user-images.githubusercontent.com/43784062/170671693-7d0c304d-b17b-475f-8b17-69aee9f934fc.jpeg)

  - Il commence par crypter les données
  - Ensuite il rajoute un entête intermédiaire sans modifier l’entête du paquet

![mode transport-1 (1)](https://user-images.githubusercontent.com/43784062/170671717-796d871d-dcfe-45f1-a1ec-e915cb5f8160.jpg)


- Fonctionnement en mode Tunnel :
 
 Ce mode permet de connectées d"uc réseaux entre eux.
 
![mode tunnel](https://user-images.githubusercontent.com/43784062/170671822-316aeaa2-8785-4c66-943a-b0c8b0f8742d.jpg)


  - Il commence par crypter le paquet contenant les données
  - Ensuite il forme un nouveau paquet avec une nouvelle entête IP pour assurer l’intégrité de celui-ci et donc empêcher un pirate de connaitre
l’expéditeur et le destinataire.

![mode tunnel -1](https://user-images.githubusercontent.com/43784062/170671863-06d7e744-6ceb-4831-9136-4cb51cbb58c2.jpg)


### IKE

IKE ou Internet Key Exchange est un procédé de gestion de clé utilisé pour gérer la connexion entre deux routeurs.

- 1. On établit une connexion sécurisée en utilisant, soit les certificats de chaque partie, soit un mot de passe commun
- 2. Une fois les routeurs d’accord sur le type de sécurité, IKE ouvre un tunnel sécurisé

![IKE](https://user-images.githubusercontent.com/43784062/170671902-771c40ec-7197-4f2e-8774-54dab0226f26.jpeg)

## Les forces et faiblesses d'IPSec

Les avantages d’IPSec sont indéniables en matière de performance et de fiabilité. 
Une fois le tunnel ouvert, les différentes formes de paquets de données (mail, ftp, voip,... ) peuvent être communiquées sans que des outils/applications n’aient à être installés.

IPSec fourni également une grande sécurité pour le trafic de données interne des entreprises mais cela peut aussi réduire la vitesse de communication.
IPsec complique la traversée des NAT et Pare-Feu.
