---
layout: default
title: DNSSEC
parent: Réseaux
---

# DNSSEC ( Domain Name System Security Extensions)

## **Qu'est-ce que le 'DNSSEC' et quelle est son histoire ?**  
###### https://en.wikipedia.org/wiki/Steven_M._Bellovin
![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Steven_M_Bellovin_2016.jpg/220px-Steven_M_Bellovin_2016.jpg)  
¹ Tout commence lorsque le [Docteur Steven M. Bellovin](https://en.wikipedia.org/wiki/Steven_M._Bellovin) a découvert des failles de sécurité dans le DNS en 1990. Il va alors publier ses découvertes en 1995, mais c'est seulement 2 ans après que le [IETF](https://www.ietf.org/), un groupe chargé d'élaborer et de promouvoir des standards Internet, va considerer ses failles de sécurité et publier la [RFC 2065](https://datatracker.ietf.org/doc/html/rfc2065) qui sera ensuite revu et corrigé pour donner la [RFC 2535](https://datatracker.ietf.org/doc/html/rfc2535) en 1999. Le DNSSec fut donc plannifier sur cette RFC  
Cette dernière était censée être complêtement fonctionnel et corrigé tous les problèmes et failles de sécurité que le Docteur Steven M. Bellovin avait précédement découvert, mais le DNSSec basé sur la RFC 2535 avait trop de difficulté à gérer le système de clé, il demandait beaucoup trop de ressources pour faire de simples échanges.  
C'est en 2005, soit 10 ans après que les travaux du Docteur Steven M. Bellovin soit rendu publique, que l'IETF publie le DNSSec-bis basé sur deux nouvelles RFCs, la [4033](https://datatracker.ietf.org/doc/html/rfc4033) et la [4035](https://datatracker.ietf.org/doc/html/rfc4035). Et ça fonctionne!  


## **Petit rappel sur le fonctionnement du DNS** 
###### https://kinsta.com
![](https://kinsta.com/fr/wp-content/uploads/sites/4/2019/02/que-sont-dns.png)

⁶ Le DNS fonctione grâce à une hiérarchisation de domaine et de sous-domaines, dont le sommet est appellé la racine et est représenté par un point.  
Chaque domaine et sous-domaine peut ensuite créer une délégation ([voir Délégation DNS](https://epheclln.github.io/Wiki-TI/R%C3%A9seaux/delegation_dns.html)) et ainsi de suite.  

  
Nous utilisons donc un résolveur pour faire des requêtes récursives, afin d'intérroger un par un, en partant de la racine, les Name Servers jusqu'a trouver l'adresse IP correspondant à notre requête.  
  
```  
Example de domaine de premier niveau : com, org, net...  
Example de domaine de second niveau : wikipedia, youtube, ephec...  
Example de sous-domaine : www, eperso, b2b...  
```  

## **Fonctionnement du DNSSEC** 
### _Chain of Trust_  
Le DNSSEC fonctione grâce à un système de chaine de confiance⁵ (en Anglais 'Chain of Trust') qui valide les Name Servers sur base d'une signature numérique basées sur la cryptographie à clé publique d'un serveur parent. Le NS pourra donc obtenir un certificat de confiance signé par le parent, et ainsi de suite jusqu'à ce que l'entièreté de la chaine soit certifié.
Le premier certificat fut approuvé par l'ICANN en 2010 pour le serveur racine.
  
Tout Résolveur voulant accéder aux données d'une zone n'aura qu'a récupérer la clé publique de cette même zone afin de valider l'authenticité des données , le résolveur confirme la signature numérique des données qu'il a récupéré. Si celle-ci sont valides , les données sont légitimes et sont renvoyées au client.Dans le cas contraire, il s'agit probablement d'une attaque et les données sont écartés et une erreur est renvoyé à l'utilisateur.


## **Pourquoi le DNSSEC est-il important ?**
⁵ Le DNSSec est très important dans la sécurisation du service DNS en général, il permet d'éviter qu'un intru se glisse dans la chaine et renvoie une autre adresse IP pour une requête, c'est ce qu'on appele du 'DNS Cache Poisoning'.  

_Qu'est-ce que le DNS Cache poisoning ?_  
L'empoisonnement du cache DNS (ou DNS Spoofing en anglais) consiste à empoisonner le cache d'un serveur DNS pour y mettre de fausses informations, de sorte que le résolveur renvoie une adresse IP incorrecte aux clients et ainsi le rediriger au mauvais endroit .
###### https://bluecatnetworks.com/
![](https://bluecatnetworks.com/wp-content/uploads/2020/10/DNS-Poisoning.png)


## **Avenir du DNSSEC ?**
Le DNSSec est très important dans la sécurisation du DNS, on a remarqué que sans celui-ci, de nombreuses fraudes et attaques pouvaient avoir lieu.  
La popularité de celui-ci augmente peu à peu chaque année³ et d'après moi le DNSSec deviendra un standard de la sécurisation, tout comme HTPPS, et tout le monde l'utilisera d'ici quelques années. 


## **Bibliographie**

*  1 : Wikipedia, [Wikipedia - Domain Name System Security Extensions ](https://en.wikipedia.org/wiki/Domain_Name_System_Security_Extensions), Apr. 2022, consulté le 26 Avril 2022

       **Résumé** : Page Wikipedia du DNSSEC
       **Avis sur la ressource** : Il s'agit de la page Wikipedia concernant le DNSSEC.
       Plus complète en Anglais, elle rassemble beaucoup d'informations concernant ce dernier. 
       
*  2 : dnssec, [ DNSSEC: DNS Security Extensions Securing the Domain Name System ](https://www.dnssec.net/), Sept. 2018, consulté le 26 Avril 2022

       **Résumé** : Page Officiel du DNSSEC
       **Avis sur la ressource** : Il s'agit de la page Officiel en anglais du DNSSEC. Même si le design n'est pas parfait, c'est toujours utile.

*  3 : Apnic,stats.lab [ Use of DNSSEC Validation for World ](https://stats.labs.apnic.net/dnssec/XA?hc=XA&hx=1&hv=0&hp=0&hr=1&w=365), consulté le 26 Avril 2022

       **Résumé** : Statistique sur l'utilisation du DNSSec
       **Avis sur la ressource** : Il s'agit d'une page du laboratoire de statistique de Apnic sur la validation DNSSec dans le monde.
       Cette ressource est utile quant à la compréhensoin de l'expansion de DNSSec.

*  4 : ICANN,stats.research [ TLD DNSSEC Report ](https://stats.research.icann.org/dns/tld_report/), consulté le 26 Avril 2022

       **Résumé** : Statistique sur les TLD signé
       **Avis sur la ressource** : Il s'agit d'une page officiel du site de l'ICANN qui représente le nombre de TLD (top-level domain, domaine de premier niveau en français) signé par DNSSEC dans le monde.Cette ressource est utile quant à la compréhensoin de l'expansion de DNSSec

*  5 : ICANN.org, [ DNSSEC – Qu'est-ce que c'est et pourquoi est-ce important ?](https://www.icann.org/resources/pages/dnssec-what-is-it-why-important-2019-03-20-fr#:~:text=Les%20DNSSEC%20renforcent%20l'authentification,par%20le%20propri%C3%A9taire%20des%20donn%C3%A9es.
), consulté le 26 Avril 2022

       **Résumé** : Page expliquant l'importance du DNSSec
       **Avis sur la ressource** : Il s'agit d'une page du site officiel de  l'ICANN, cette ressource est donc cohérente.

*  6 : Wikipedia, Domain Name System, [ Domain Name System ](https://fr.wikipedia.org/wiki/Domain_Name_System), consulté le 26 Avril 2022

       **Résumé** : Page Wikipedia du DNS
       **Avis sur la ressource** : Plus complête en Anglais, pratique pour se rappeller de petits détails. 
         
Tommy Riquet  
Dernière modification , le 26 Avril 2022
