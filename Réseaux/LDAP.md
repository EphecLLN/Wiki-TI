[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# LDAP


Aujourd’hui, l’un des enjeux majeures lors de la croissance d’une entreprise est la sécurité de son systeme d'information; il est impératif de garder le contrôle sur l’ensemble des données nécessaires à l’authentification et les droits d’accès…ces informations sont très difficiles à maitriser car très volatiles et éparses, il devient donc de plus en plus difficile d’utiliser un stockage de données classique. Une solution à ce problème serait de mettre en place des annuaires LDAP.
LDAP (Lightweight Directory Access Protocol) est un protocole qui spécifie une méthode de stockage d’annuaire et facilite l’authentification et l’autorisation des utilisateurs aux serveurs, fichiers, équipements réseaux etc...


## LES ORIGINES DU PROTOCOLE LDAP
LDAP a été codéveloppé par Tim Howes de l'Université du Michigan, Steve Kille d' Isode Limited et Wengyik Yeong de Performance Systems International, en 1993. [[1](https://ldapwiki.com/wiki/History%20of%20LDAP)].
Au commencement, l'Union Internationale des Télécommunications (UIT) créa les annuaires X.500 qui reposaient sur le protocole DAP (directory access protocol) . Ce fut à l’époque un concept novateur dans l’uniformisation d’accès aux services, la centralisation et la protection des ressources. Seulement il était très difficile à mettre en place a la fois pour les systèmes et pour les réseaux. LDAP apparaitra comme une solution a tous ces problèmes en permettant l’authentification et l’autorisation des utilisateurs sur les serveurs, les fichiers et les applications tout en réduisant la bande passante et la demande sur les terminaux. Grâce à ces gains d'efficacité, LDAP connaîtrait un grand succès et deviendra le protocole d'authentification de facto des services d'annuaire Internet pendant un certain temps et en 1997, LDAPV3 a été proposé et acceptée comme norme internet pour les services d’annuaire ; aujourd’hui encore c’est la version la plus recente et la plus rependue

[[2](https://openclassrooms.com/fr/courses/2257706-presentation-du-concept-dannuaire-ldap)]


## DESCRIPTION DU PROTOCOLE LDAP
LDAP est un protocole de la couche 7 qui utilise TCP comme protocole de transport et fonctionne par défaut sur le port 389 et 636 pour sa version sécurisée (LDAP OVER TLS/SSL).

On distingue 02 méthodes de communications :
- **Client/serveur** : accès aux informations par les clients
- **Serveur/serveur** : duplication des informations entre serveurs


 La version actuelle (version 3) proposent les évolutions suivantes :
- 	L'utilisation de l'encodage UTF-8
-  L'authentification via Simple Authentication and Security Layer (SASL), et Transport Layer Security (TLS)
-  Le support des Referrals (une branche pointe vers un autre annuaire)
-  La capacité d'étendre le protocole
-  Le support des schémas dans l'annuaire

## AVANTAGES ET INCONVENIENTS DE LDAP

### AVANTAGES
- 	Il peut être open source : on a par exemple OPENLDAP 
- 	Standardisé :  LDAP a été ratifié en tant que norme IETF (Internet Engineering Task Force) en 1997 avec RFC 2251. [[3](https://www.journaldunet.fr/web-tech/dictionnaire-du-webmastering/1203397-ldap-lightweight-directory-access-protocol-definition-traduction)].

- 	Flexible : l’authentification LDAP est utilisé pour de nombreux cas de figure.
- 	Securisé : en effet grâce à SSL ou TLS, il est tout à fait possible de chiffrer les communications LDAP et donc les rendre plus ou moins inaccessibles 
- Multi-système : LDAP est compatible avec différents systèmes d’exploitation et d’appareils.  Cependant, différentes offres LDAP peuvent limiter cette flexibilité ; active directory, par exemple, est centré sur Windows et necessite souvent des options complémentaires pour fonctionner avec des systèmes d’exploitation supplémentaires.


### INCONVENIENTS
- Ancienneté : LDAP est un protocole plus ancien. Aujoud’hui il existe de nouveaux protocoles d’authentification tels que SAML qui sont conçus pour les environnements informatiques modernes orientés cloud.[[4](https://www.okta.com/fr/blog/2016/12/what-is-saml/)].
- 	Mise en place assez complexe : l’installation et la maintenance de LDAP dans une entreprise nécessite généralement des compétences assez poussées.

## PRINCIPES

Un serveur LDAP agira donc de façon **asynchrone** comme un **intermédiaire** entre un client et une source de données qui pourrait être un fichier ou une base de données. Ses principales fonctions sont:
- **Mise à jour** : Cela inclut l'ajout, la suppression ou la modification des informations de répertoire.
- **Requête** : Cela inclut la recherche et la comparaison des informations d'annuaire.
- **Authentification** : les principales fonctions d'authentification incluent la liaison et la déliaison ; une troisième fonction, abandon, peut être utilisée pour empêcher un serveur de terminer une opération. 

![image](https://user-images.githubusercontent.com/64273779/168279400-381b5146-817d-4bc3-a4c5-85a5499e780a.png)


les modèles LDAP représentent les services que propose le serveur au client. On en distingue 04 d’après) qui sont :

- **Le modèle de nommage** : définit La façon dont les informations sont stockées et organisées dans l’annuaire.
- **Le modèle fonctionnel** : définit les services fournis par l’annuaire (recherche , ajout).
- **Le modèle d’information** : définit le type d’informations stockées.
- **Le modèle de sécurité** : définit les droits d’accès aux ressources[[5](https://openclassrooms.com/fr/courses/2257706-presentation-du-concept-dannuaire-ldap)]



### LE MODELE DE NOMMAGE

Dans un annuaire LDAP, les données sont représentées de manière hiérarchique, on parlera ici d’arborescence d’informations d’annuaire (DIT). Dans cette structure arborescente, chaque élément est appelé une entrée et possède :
+  Un ensemble d’attributs qui représentent les caractéristiques de ce dernier.
+ un DN (Distinguished Name) qui est le nom complet permettant de le situer dans l’arbre. 
+	Un RDN (relative distinguished Name ) qui est la partie du DN permettant de représenter cette élément de manière unique dans l’annuaire.

Toutes les entrées n’ayant pas de subordonées correspondent donc à des feuilles et la racine est l’entité globale qui englobe toutes les informations dans l’annuaire.

![image](https://user-images.githubusercontent.com/64273779/168279486-c4a11c9f-1f0f-4231-a445-b73a83cb3e17.png)


Dans l’Example ci-dessus, la racine correspond au nom de domaine où est hébergé le serveur LDAP. On a donc 02 branches (users et groups) qui correspondent aux organisational units.

La feuille BRICE  par exemple aura comme DN : **cn=brice,ou=users,dc=ephec-ti,dc=be et son  RDN sera brice**


### LE MODELE FONCTIONNEL
Il décrit les moyens d’acces et les operations applicables aux données.

Les operations possibles seront :
- 	Opération d’interrogation : requête pour accéder aux données
- 	Opération de comparaison :  renvoie vrai ou faux si egal.
- Les operations de mise a jour :
   - 	Add : ajouter une entrée au repertoire
   -  Delete : suppromer une entrée au repertoire
   - 	Rename : modifier le nom d’entrée d’un repertoire
   - 	Modify : modifier une entrée
- Les opérations d’authentification et de contrôle : 
   - 	Bind : Initier une nouvelle session sur le serveur LDAP
   - 	Unbind : Terminer une session sur le serveur LDAP
   - 	Abandon : Abandonner l’operation précédemment envoyée au serveur.

   ### LE MODELE D’INFORMATION 

   Il définit le type de données pouvant être stocké dans l’annuaire.
   Une Entrée représente l’élément de base de l’annuaire. Elle contient les données, un ensemble d’attributs ; on peut faire le parallèle avec les classes en POO.

   Un attribut pourra donc appartenir à plusieurs classes. Il est caractérisé par un nom, un type, une méthode de comparaison, un objet Identifier, une valeur.

   Une classe d’objet est définie par :
   *  	Un nom
   * Un OID qui l’identifie de manière unique dans l’arborescence
   *  	Des attributs obligatoires
   *  	Des attributs optionnels 

L’ensemble formé par ces classes d’objet et leurs attributs seront définis par un schéma.


### LE MODELE DE SECURITE
Il décrit le moyen de protéger les données de l’annuaire par des mécanismes permettant aux clients de s’identifier et d’acceder aux données selon leurs droit. Il s’etend sur plusieurs niveaux : 
- 	L’authentification pour se connecter au service : Pour accéder aux ressources de l’annuaire, le client doit s’authentifier auprès du serveur. Cette opération est dite de « binding » [[6](https://openclassrooms.com/fr/courses/2257706-presentation-du-concept-dannuaire-ldap/2260351-modele-de-securite)]
   les différentes méthodes d’authentification proposée par LDAPV3  sont:
   * 	Anonymous authentification : accès sans authentification
   * 	Root DN authentication : accès administrateur
   *  Mot de passe+ SSL ou TLS : accès chiffré
   *  Certificats sur SSL : échange de clé publique ou privée
-  Un modèle de contrôle d’accès aux données : droit d’accès aux données(lecture, ecriture, recherche , comparaison).
-  Par le chiffrement des communications : utilisation de SSL ou TLS


## MISE EN PLACE D'UN SERVEUR LDAP

La configuration d’un annuaire LDAP passe par plusieurs étapes a savoir :

### Etape1 : **la phase de conception**

On determine:
+ 	La nature des données
+ 	La finalité de ces données c’est-à-dire ce que l’on compte en faire
+ 	La manière de gérer ces données
+ 	On désigne enfin le DIT (DIRECTORY INFORMATION TREE)

### Etape2 : **configuration du serveur**

+  Installation du serveur
+ 	Creation du/des comptes administrateurs
+ 	Ajout des nouveaux objets en fonction du DIT designé au prealable

### ETAPE 3 : **sécurisation du service**

Configuration des ACL : définir  les types d’utilisateurs et les niveaux d’accès pour chaque ressource de l’annuaire. 

on distingue plusieurs d'accès:

| niveau    |   Description  |  
| ------------- |------------- | 
| none   |        pas d'accès      |     
| compare        |comparaison  |      
| auth  |      authentification     |    
|search |   recherche    |  
| read |        lecture    |   
| write |      ecriture  |   


Les différents types d’utilisateurs


| niveau    |   Description  |  
| ------------- | ------------- | 
| *  |      Tous les types d’utilisateurs     |     
|anonymous       |Utilisateur anonyme  |      
| users  |    Utilisateur authentifié   |    
|self|  Utilisateur associé à l’entrée recherche|  

[[7](http://igm.univ-mlv.fr/~dr/XPOSE2009/ldap/xpose_ldap.pdf)]


## LDAP VERS LE CLOUD?

Aujourd'hui la plupart des entreprises se tournent vers des solutions CLOUD; il nait donc le besoin de solutions d'annuaire plus flexibles, pouvant fonctionner avec des ressources cloud et s'adapter à differents systemes d'exploitations. Les solutions centrées sur Microsoft windows et Azure d'active directory commencent donc à présenter des limites pour de nombreuses entreprises avec des systemes d'exploitation divers. Ces nouveaux défis avec AD et les annuaires sur site ont poussé les entreprises à se tourner vers le cloud LDAP et, finalement, vers Directory-as-a-Service.





## Bibliographie

*  [[1](https://ldapwiki.com/wiki/History%20of%20LDAP)].
   - titre : History of LDAP
   - auteur:  
   - derniere modification: 20/06/ 2019
   - date de consultation : 11/05/2022

*  [[2](https://openclassrooms.com/fr/courses/2257706-presentation-du-concept-dannuaire-ldap)]
   - titre: presentation du concept d'annuaire LDAP
   - auteur: Antoine Bouet
   - derniere mise à jour: 14/02/2022
   - date de consultation: 11/05/2022

*  [[3](https://www.journaldunet.fr/web-tech/dictionnaire-du-webmastering/1203397-ldap-lightweight-directory-access-protocol-definition-traduction)]
   - titre : LDAP (Lightweight Directory Access Protocol) : définition, traduction
   - auteur: 
   - derniere mise à jour : 09/01/2019
   - date de consultation : 12/05/2022

* [[4](https://www.okta.com/fr/blog/2016/12/what-is-saml/)]
    - titre : Qu'est-ce que le protocole SAML ?
   - auteur: Ed Sawma
   - derniere mise à jour : 04/09/2020
   - date de consultation : 12/05/2022

* [[5](https://openclassrooms.com/fr/courses/2257706-presentation-du-concept-dannuaire-ldap)]
   - titre: presentation du concept d'annuaire LDAP
   - auteur: Antoine Bouet
   - derniere mise à jour: 14/02/2022
   - date de consultation: 11/05/2022

*   [[6](https://openclassrooms.com/fr/courses/2257706-presentation-du-concept-dannuaire-ldap/2260351-modele-de-securite)]
   - titre: presentation du concept d'annuaire LDAP
   - auteur: Antoine Bouet
   - derniere mise à jour: 14/02/2022
   - date de consultation: 11/05/2022

*  [[7](http://igm.univ-mlv.fr/~dr/XPOSE2009/ldap/xpose_ldap.pdf)]
   - titre: LDAP et les services d'annuaire 
   - auteur: marc olory
   - derniere mise a jour: 12/12/2010
   - date de consultation: 13/05/2022

   
   

