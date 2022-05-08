---
layout: default
title: DNSSEC
parent: Réseaux
---

# DNSSEC ( Domain Name System Security Extensions)

## Histoire du DNSSEC 

###### https://en.wikipedia.org/wiki/Steven_M._Bellovin
![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Steven_M_Bellovin_2016.jpg/220px-Steven_M_Bellovin_2016.jpg) 

 Tout commence en 1990, lorsque le [Docteur Steven M. Bellovin](https://en.wikipedia.org/wiki/Steven_M._Bellovin) a découvert des failles de sécurité dans le protocole DNS¹. Il a remarqué qu'il était très facile pour une personne malveillante de rediriger un utilisateur vers un autre site web. 

Il faut savoir que le DNS n'avait aucun système de sécurité car Internet était très loin d'être le géant qu'il est aujourd'hui et le seul moyen pour un résolveur de vérifier les informations qu'il recevait était de comparer l'adresse IP source d'une réponse avec l'adresse IP de la requête, ce qui n'est pas du tout sécurisé de part la facilité à falsifier celles-ci.
 
Les trois principales failles de sécurité découvertes par le [Docteur Steven M. Bellovin](https://en.wikipedia.org/wiki/Steven_M._Bellovin) sont le "DNS Spoofing", le "DNS Cache poisoning" et la "man-in-the-middle attack". 
  
**Qu'est-ce que le DNS Spoofing / DNS Cache Poisoning⁵ ?**   
Le DNS Spoofing (usurpation DNS en français) est l'action de rediriger un utilisateur vers un site web malveillant afin de lui soutirer des informations privées, telles que des mots de passe ou des numéros de carte de crédit.  
Les attaquants se font passer pour le serveur autoritaire d'un site web afin d'empoisonner le cache d'un résolveur pour y mettre de fausses informations, de sorte que celui-ci renvoie une adresse IP incorrecte aux utilisateurs et ainsi les redirige au mauvais endroit.

###### BLUECATNETWORKS, _Kyle Roblyer_ , 25 Novembre 2019, [en ligne] https://bluecatnetworks.com/blog/what-is-dns-poisoning-how-to-prevent-it/
<img src="https://bluecatnetworks.com/wp-content/uploads/2020/10/DNS-Poisoning.png"  height="400" />

**Qu'est-ce que l'Attaque de l'homme du milieu⁹ ?**  
L'attaque de l'homme du milieu (man-in-the-middle attack en anglais) n'a rien à voir avec la terre du même nom, mais consiste plutôt à intercepter/relayer la communication entre deux parties.
L'attaquant a donc le contrôle direct sur les informations et peut donc décider de les modifier à sa guise. Cette attaque est très souvent utilisée pour rediriger des utilisateurs lambda vers des sites web frauduleux.

###### MALWAREBYTES, Blog, [en ligne] https://blog.malwarebytes.com/101/2018/07/when-three-isnt-a-crowd-man-in-the-middle-mitm-attacks-explained/
<img src="https://blog.malwarebytes.com/wp-content/uploads/2018/07/shutterstock_758712814-900x506.jpg"  height="300" />
<br>

### **Et comment peut-on empêcher ces attaques ?**  
Ces attaques ont tous un point en commun en terme de sécurité, c'est que le résolveur n'a aucun moyen de savoir si les informations qu'il reçoit sont correctes car il ne sait pas avec quelle machine il communique. Il nous faut donc un système pour authentifier les machines DNS afin d'empêcher quiconque de s'immiscer entre les mailles du filet .  
C'est ainsi que le DNSSEC fut inventé en se basant sur la cryptographie à clé publique .
  

## **Petit rappel sur le fonctionnement du DNS⁶** 
Le but du protocole DNS est de mettre en oeuvre des mécanismes afin de faire correspondre un nom de domaine et une adresse IP.  
Celui-ci fonctionne grâce à une hiérarchisation de domaines et de sous-domaines, dont le sommet est appelé la racine et est représenté par un point.  
Chaque domaine et sous-domaine peut ensuite créer une délégation ([voir Délégation DNS](https://epheclln.github.io/Wiki-TI/R%C3%A9seaux/delegation_dns.html)) et ainsi de suite.  


Nous utilisons donc un résolveur pour faire des requêtes récursives, afin d'interroger un par un, en partant de la racine, les "Name Servers" jusqu'à trouver l'adresse IP correspondante à notre requête.  
  

![](https://www.nameshield.com/wp-content/uploads/2020/03/DNS-768x419.png)
###### NameSchield, [en ligne] https://www.nameshield.com/ressources/lexique/dns-domain-name-system/

 
Prenons un exemple avec 'www.wikipedia.org';
- Notre Internaute veut accéder à la page Internet avec l'url 'www.wikipedia.org', il va alors interroger son résolveur pour lui demander l'adresse IP du Serveurs autoritaire de 'wikipedia.org'
- La première requête de notre résolveur va nous faire interroger le serveur Racine, qui va nous renvoyer l'adresse IP du NS '.org'.
- La deuxième requête va ensuite interroger le NS '.org' qui va nous renvoyer l'IP du NS 'wikipedia.org' 
- La troisième requête va ensuite interroger le NS de 'wikipedia.org' qui va nous répondre avec l'adresse IP de 'www.wikipedia.org'. 
- Notre résolveur renvoie l'adresse IP du serveur autoritaire de wikipedia.org à notre Internaute.



## **Fonctionnement du DNSSEC**  
### **Sécurisation d'une zone⁸**
- Tout d'abord, des Ressource Records de même type (A,AAAA,MX...) sont regroupés ensemble dans un RRset. 
- Chaque Serveur DNS génère une paire de clés ZSK (**Z**one **S**ignature **K**ey) et une paire de clés KSK(**K**ey **S**ignature **K**ey).
- La ZSK privée est utilisée pour signer le RRset et stocke la signature dans un RRSIG.   

<br><img src="https://www.cloudflare.com/img/products/ssl/diagram-rrsets.svg" width="300" height="200" /> 
<br><img src="https://www.cloudflare.com/img/products/ssl/diagram-zone-signing-keys-1.svg" width="300" height="200" /> 

###### CLOUDFLARE, [en ligne] https://www.cloudflare.com/fr-fr/dns/dnssec/how-dnssec-works/

- La ZSK publique est stockée dans un Ressource Record "DNSKEY" et utilisée pour vérifier la signature .  
- La KSK publique est stockée dans un Ressource Record "DNSKEY" .  
- Ces deux RR 'DNSKEY' sont rassemblées dans un RRset .
- La KSK privée est utilisée pour signer le RRset "DNSKEY" et stocke la signature dans un autre RRSIG .  

<br><img src="https://www.cloudflare.com/img/products/ssl/diagram-key-signing-keys-1.svg" width="300" height="200" />

###### CLOUDFLARE, [en ligne] https://www.cloudflare.com/fr-fr/dns/dnssec/how-dnssec-works/
  <br>

Pour l'instant, le serveur de nom agit de manière indépendante, il faut trouver un moyen de propager la confiance entre les serveurs .  
C'est ce qu'on appelle la _Chain of Trust_.


### **Chain of Trust⁷**   
Le DNSSEC fonctionne à l'aide d'un système de chaîne de confiance⁵ (en Anglais 'Chain of Trust') qui valide les serveurs DNS sur base de leurs RRSIG. Le NS pourra donc obtenir un certificat de confiance signé par le parent, et ainsi de suite jusqu'à ce que l'entièreté de la chaîne soit certifiée.
Lorsque une zone enfant termine sa sécurisation, elle hache son RRset DNSKEY et l'envoie à la zone parent pour qu'elle l'ajoute en tant qu'enregistrement DS(Delegation Signer) dans ses Ressources Records. Celui-ci va ensuite repasser dans le processus de sécurisation de la zone parent . Et ainsi de suite jusqu'au "Root Server", dont le certificat fut approuvé manuellement par l'ICANN en 2010.    
<br>
**Exemple de requête :**  
###### RessellersPanel, [en ligne] , https://blog.resellerspanel.com/domain-names/dnssec-enabled-on-our-platform.html
![](https://blog.resellerspanel.com/wp-content/uploads/2017/02/dnssec-ds-records.jpg)  
- Un utilisateur essaie de joindre "www.DOM.com", il va alors contacter la Zone Racine
- La Zone Racine va alors vérifier la zone ".com" avec sa KSK publique 
- La zone ".com" va vérifier le RR de la zone "DOM.com"
- La zone "DOM.com" va finalement vérifier le RR de "www.DOM.com" 
- Le RR est ensuite renvoyé à l'utilisateur 


### **Ressource Record DNSSEC**  
Exemple de Ressource Record 'DNSKEY' :
```
ephec.com. 3600 IN DNSKEY 257 3 13 gkfdFDSdsfl21VC== 
```

| Nom | TTL | Class | Type de RR | Flags | Protocole | Algorithme | Clé Publique |
|---|---|---|---|---|---|---|---|
| ephec.com. | 3600 | IN | DNSKEY | 257 | 3 | 6 | gkfdFDSdsfl21VC==  |  

- Les 4 premiers champs définissent le nom de domaine de la zone à qui appartient la clé publique 
- Le champ "Flags" indique des informations supplémentaires 
- Le champ "Protocole" est toujours égal à 3 pour le DNSSEC 
- Le champ "Algorithme" correspond à l'algorithme utilisé pour le chiffrement de la clé publique  

Exemple de Ressource Record 'RRSIG' :
```
ephec.com. 3600 IN RRSIG A 5 2 fd3Fsd5G43dsqlm=) 
```

| Nom | TTL | Class | Type de RR | Type Covered | Algorithme | Nombre de label | Signature |
|---|---|---|---|---|---|---|---|
| ephec.com. | 3600 | IN | RRSIG | A | 5 | 2 | fd3Fsd5G43dsqlm=)   |  

- Les 4 premiers champs définissent le nom, le TTL(Time to Leave), la classe et le type de Ressource Record
- Le champ "Type Covered" indique le type de RR couvert par la signature 
- Le champ "Algorithme" indique l'algorithme utilisé pour créer la signature 
- Le champ "Nombre de label" correspond au nombre de label dans le nom => "ephec.com" = 2 (ephec / com) 

Exemple de Ressource Record 'DS' :
```
ephec.com. 3600 IN DS 2371 13 2 3K12J312LK321L 
```

| Nom | TTL | Class | Type de RR | Tag de clé | Algorithme | Type de hash | Valeur du hash |
|---|---|---|---|---|---|---|---|
| ephec.com. | 3600 | IN | DS | 2371 | 13 | 2 | 3K12J312LK321L   |  

- Les 4 premiers champs définissent le nom, le TTL(Time to Leave), la classe et le type de Ressource Record
- Le champ "Tag de clé" indique un nombre qui identifie la référence du record DNSKEY
- Le champ "Algorithme" indique l'algorithme utilisé pour le RR DNSKEY 
- Le champ "Type de hash" correspond à l'algorithme de hash cryptographique utilisé pour créer le hash 


## **Avenir du DNSSEC ?**
Le DNSSec est très important dans la sécurisation du DNS. 
On a remarqué que sans celui-ci, de nombreuses fraudes et attaques pouvaient avoir lieu.  
Sa popularité augmente peu à peu chaque année³ et d'après moi le DNSSec deviendra un standard de la sécurisation, tout comme HTPPS. 



<br><br>

## **Bibliographie**
   

*  1 : Wikipedia, [Wikipedia - Domain Name System Security Extensions ](https://en.wikipedia.org/wiki/Domain_Name_System_Security_Extensions), Apr. 2022, consulté le 26 Avril 2022

       **Résumé** : Page Wikipedia du DNSSEC
       **Avis sur la ressource** : Il s'agit de la page Wikipedia concernant le DNSSEC.
       Plus complète en Anglais, elle rassemble beaucoup d'informations concernant ce dernier. 
       
*  2 : dnssec, [ DNSSEC: DNS Security Extensions Securing the Domain Name System ](https://www.dnssec.net/), Sept. 2018, consulté le 26 Avril 2022

       **Résumé** : Page Officiel du DNSSEC
       **Avis sur la ressource** : Il s'agit de la page officiel en anglais du DNSSEC. Même si le design n'est pas parfait, c'est toujours utile.

*  3 : Apnic,stats.lab [ Use of DNSSEC Validation for World ](https://stats.labs.apnic.net/dnssec/XA?hc=XA&hx=1&hv=0&hp=0&hr=1&w=365), consulté le 26 Avril 2022

       **Résumé** : Statistique sur l'utilisation du DNSSec
       **Avis sur la ressource** : Il s'agit d'une page du laboratoire de statistique de Apnic sur la validation DNSSec dans le monde.
       Cette ressource est utile quant à la compréhension de l'expansion de DNSSec.

*  4 : ICANN,stats.research [ TLD DNSSEC Report ](https://stats.research.icann.org/dns/tld_report/), consulté le 26 Avril 2022

       **Résumé** : Statistique sur les TLD signé
       **Avis sur la ressource** : Il s'agit d'une page officielle du site de l'ICANN qui représente le nombre de TLD (top-level domain, domaine de premier niveau en français) signé par DNSSEC dans le monde. Cette ressource est utile quant à la compréhension de l'expansion de DNSSec

* 5 : ICANN.org, [ DNSSEC – Qu'est-ce que c'est et pourquoi est-ce important ?](https://www.icann.org/resources/pages/dnssec-what-is-it-why-important-2019-03-20-fr#:~:text=Les%20DNSSEC%20renforcent%20l'authentification,par%20le%20propri%C3%A9taire%20des%20donn%C3%A9es.
), consulté le 26 Avril 2022

       **Résumé** : Page expliquant l'importance du DNSSec
       **Avis sur la ressource** : Il s'agit d'une page du site officiel de l'ICANN, cette ressource est donc cohérente.

* 6 : Wikipedia, Domain Name System, [ Domain Name System ](https://fr.wikipedia.org/wiki/Domain_Name_System), consulté le 26 Avril 2022     
 
       **Résumé** : Page Wikipedia du DNS
       **Avis sur la ressource** : Plus complête en Anglais, pratique pour se rappeler de petits détails.

*  7 : ResellersPanel, DNSSec Enabled On our Platform, [ DNSSec Enabled on our platform ](https://blog.resellerspanel.com/domain-names/dnssec-enabled-on-our-platform.html), consulté le 28 Avril 2022

       **Résumé** : Page rassemblant plein d'informations sur le DNSSec
       **Avis sur la ressource** : En Anglais, très complête avec beaucoup d'illustrations  

*  8 : CloudFlare, [ How DNSSEC Works](https://www.cloudflare.com/fr-fr/dns/dnssec/how-dnssec-works/), consulté le 28 Avril 2022

       **Résumé** : Explication sur l'utilisation des clés et le fonctionnement du DNSSEC
       **Avis sur la ressource** : Très complète avec plein d'illustrations

*  9 : Wikipedia, [Man-in-the-Middle Attack](https://fr.wikipedia.org/wiki/Attaque_de_l%27homme_du_milieu), consulté le 7 Mai 2022

       **Résumé** : Explication Sur l'attaque de l'homme du milieu
       **Avis sur la ressource** : Très complète avec des d'exemples



