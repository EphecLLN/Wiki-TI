# Mise en œuvre pratique de DNSSEC: Implémentation (Bind9) et tests associés + récapitulatif DNS

## Récapitulatif
### Le DNS en entreprise

Le réseau d'une entreprise comprend généralement, le résolveur placé en interne/Trusted zone avec un SOA interne et un SOA externe placé dans la DMZ.

Le rôle principal de résolveur placé en interne/Trusted zone est de recevoir les requêtes venant des utilisateurs et de les transmettre aux SOA approprié pour obtenir les réponses voulues. Il va interroger le SOA interne pour résoudre les noms de domaine internes, par exemple lorsque des clients envoient un requête pour le serveur web interne (ou autre), afin d'obtenir l'adresse ip qui correspond. Cela permet aux utilisateurs en interne d'accéder directement à ces services sans passer par internet et offrir du coup une meilleure performance et sécurité.
Le SOA interne permet donc de gérer les zones DNS internes, qui contiennent les infos spécifiques à l'entreprise. Il répond aux requêtes DNS internes en résolvant les noms de domaine internes et fournis les réponses adéquates. En plaçant ce serveur dans le réseau interne, ça permet de limiter l'accès aux informations sensibles et de renforcer la sécurité globale du réseau.

Quant à lui, le SOA externe est placé en DMZ. Pour rappel la DMZ est une zone conçue pour isoler les services accessibles publiquement.
Le SOA externe gère les zones DNS publiques de l'entreprise et est utilisé pour les services accessibles depuis l'Internet. Il répond aux requêtes DNS externes provenant des clients externes. En plaçant celui-ci dans la DMZ, on réduit les menaces et vulnérabilités car les attaquants ne peuvent pas accéder directement au réseau interne de l'entreprise.

### Les types de requêtes: 
Il y'a trois types de requêtes DNS :

1. Récursive: Globalement, quand un pc ou un résolveur effectue une requête récursive, il demande au serveur DNS de lui fournir directement la réponse complète. Cela veut dire que le serveur DNS interroge les autres serveurs DNS de manière itérative jusqu’à quand il obtient la réponse souhaitée.

2. Itérative: la personne/pc qui fait la requête demande au serveur DNS de lui fournir la meilleure réponse qu'il possède, même si ce n'est pas la réponse complète. Si le serveur DNS ne trouve pas l'info demandée, il renverra une indication sur le serveur de noms ayant l'autorité sur le domaine. Le demandeur devra alors continuer à interroger des autres serveurs DNS pour obtenir la réponse complète.

3. Non récursive: Généralement utilisé par les résolveurs DNS pour chercher une IP spécifique qui n'est pas présente dans leur cache. Le résolveur envoie une requête à un serveur DNS spécifique et se contente d'une seule réponse. Il ne poursuit pas le processus de requête récursive pour obtenir une réponse complète, cela permet de limiter l'utilisation de la bande passante.

### Gestion des noms de domaine public et réservation via les registars :

Quand une entreprise souhaite réserver un nom de domaine accessible publiquement, elle doit passer par un registar (permet de gérer et réserver des noms de domaine) autorisé par le registry du TLD (Top Level Domain) qui correspond à son choix de nom de domaine. Les registars sont très important car ils permettent la gestion des noms de domaine en facilitant la réservation et le renouvellement des noms de domaine. C'est en fait des intermédiaires entre les entreprise et les registries (Registry: base de donnée de tout les sous domaines d'un TLD).

### Délégation de zone

Délégation de zone :

_"La délégation DNS est le processus par lequel un serveur DNS autorise un autre serveur DNS à effectuer des actions faisant autorité au nom de ce serveur. Cela permet une utilisation plus efficace des ressources, en particulier lorsqu'il s'agit de déploiements à grande échelle."_ (1)

C'est un mécanisme qui permet de diviser la gestion du DNS en différentes zones responsables de sous-domaines. Quand une entreprise va réserver un nom de domaine publique, elle doit configurer la délégation de zone pour indiquer au système DNS publique comment trouver la zone correspondante à son nom de domaine. Il faut pour cela ajouter des enregistrements NS au domaine parent.
Exemple: `domaineParent.com.   IN   NS   ns1.domaineParent.com.`

### Menaces courantes liées au DNS et mesures de protection (DNSSEC)

Parmi les menaces courantes liées au DNS, on retrouve:

* Cache poisoning: 

_"le cache poisoning est une technique permettant de leurrer les afin de leur faire croire qu'ils reçoivent une réponse valide à une requête qu'ils effectuent, alors qu'elle est frauduleuse."_ (4)

C'est en fait une attaque dans laquelle un attaquant modifie les infos stockées dans le cache du serveur DNS. L'objectif est de redirigé le trafic vers des sites Web malveillants. L'attaquant envoie des fausses réponses DNS au serveur DNS cible en les faisant passer pour des réponses légitimes provenant de serveurs DNS autoritaires. L'attaquant force donc le serveur DNS à stocker ces fausses informations dans son cache.

* Pharming:

Même principe que le cache poisoning, l'attaquant redirige le trafic d'un utilisateur vers un site Web malveillant en modifiant les paramètres DNS mais généralement utilisé pour voler des informations telles que les identifiants de connexion/données personnelles.

* ID spoofing:

Cette attaque vise à tromper un serveur DNS en modifiant l'identifiant (ID) des requêtes DNS. D'habitude, quand un client envoie une requête DNS à un serveur, celui-ci attribue un ID unique à cette requête. L'ID est utilisé pour faire correspondre les réponses aux requêtes correspondantes.
Lors d'une attaque d'ID spoofing, un attaquant essaie de prédire ou de deviner l'ID d'une requête DNS valide pour fournir une fausse réponse avant que le serveur DNS n'ait la chance de recevoir la véritable réponse. 
L'objectif reste de tromper les utilisateurs et de les rediriger vers des sites Web malveillants de l'attaquant.

* DoS: 

Attaque qui a pour but de rendre un service indisponible en submergeant le serveur DNS de requêtes.

* Tunneling DNS:

Méthode qui permet de contourner les firewalls en utilisant le trafic DNS pour établir des communications avec des machines infectées derrière le firewal en contournant la sécuritée.


Pour contrer ces menaces, des mesures de protection doivent être mises en place. Une solution efficace est l'utilisation du DNSSEC.

## Configuration de DNSSEC dans Bind9

### DNSSEC c'est quoi ?

Le DNSSEC est une sécuritée qui ajoute des mécanismes de protection supplémentaires au DNS. Il utilise des signatures pour garantir "l'intégrité et l'authenticité des données DNS".

En utilisant DNSSEC, les entreprise peuvent renforcer la sécurité de leurs infrastructures DNS et protéger les utilisateurs contre les attaques malveillantes. C'est pourquoi je vais expliquer comment configurer DNSSEC sur bind9...

### Configuration du DNSSEC (SOA externe)

Voici les étapes pour configurer DNSSEC sur bind9 de manière éfficace.

1. Avant tout, il faut activer DNSSEC dans les `options{ }` de votre fichier `named.conf.options` où `named.conf` en fonction de votre configuration:
* Ouvrir avec l'éditeur nano le fichier contenant les options de named.conf: `nano /etc/bind/named.conf.options` et insérer les lignes suivantes dans les options: 
```
dnssec-enable yes;
dnssec-validation yes;
dnssec-lookaside auto;
```

2. Créer une ZSK (Zone Signing Key) et une KSK (Key Signing Key): 

* ZSK: Utilisée pour signer les enregistrements de ressources dans la zone de domaine donnée, _"assurant ainsi l'intégrité des données et la validité des réponses DNS."_ (23)

`dnssec-keygen -a NSEC3RSASHA1 -b 2048 -n ZONE example.com`

KSK: signature plus puissante utilisée pour signer la ZSK

`dnssec-keygen -f KSK -a NSEC3RSASHA1 -b 4096 -n ZONE example.com`

3. Le répertoire aura maintenant 4 clés - des paires privées/publiques de ZSK et KSK. Il faut maintenant ajouter les clés publiques qui contiennent l'enregistrement DNSKEY au fichier de zone. On va utiliser cette boucle:
```
for key in `ls Kexample.com*.key`
do
echo "\$INCLUDE $key">> example.com.zone
done
```

4. Maintenant que les clés sont ajoutées au fichier de zone, on va signer la zone avec cette commande :

`dnssec-signzone -3 $(head -c 1000 /dev/random | sha1sum | cut -b 1-16) -A -N INCREMENT -o <zonename> -t <zonefilename>`

L'output devrait ressembler à ceci:
```
root@ubuntu:/var/cache/bind# dnssec-signzone -A -3 $(head -c 1000 /dev/random | sha1sum | cut -b 1-16) -N INCREMENT -o l2-4.ephec-ti.be -t l2-4.ephec-ti.be.zone
Verifying the zone using the following algorithms: NSEC3RSASHA1.
Zone signing complete:
Algorithm: NSEC3RSASHA1: KSKs: 1 active, 0 stand-by, 0 revoked
                        ZSKs: 1 active, 0 stand-by, 0 revoked
example.com.zone.signed
Signatures generated:                       14
Signatures retained:                         0
Signatures dropped:                          0
Signatures successfully verified:            0
Signatures unsuccessfully verified:          0
Signing time in seconds:                 0.046
Signatures per second:                 298.310
Runtime in seconds:                      0.056
```

5. Charger la zone signée: 

* Modifier le fichier `named.conf.local` où `named.conf` en fonction de votre configuration:


`nano /etc/bind/named.conf.local`

* Changer l'option file de la zone concernée: 

```
zone "example.com" IN {
    ///// zone option /////
    file "example.com.zone.signed";
};
```

6. Redémarrer Bind9:

`sudo systemctl restart bind9 `


## Tests de DNSSEC

Option 1: `Dig`

`dig A example.com.`

```
; <<>> DiG 9.8.4-rpz2+rl005.12-P1 <<>> A example.com. @localhost +noadditional +dnssec +multiline
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32902
;; flags: qr aa rd; QUERY: 1, ANSWER: 2, AUTHORITY: 3, ADDITIONAL: 5
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags: do; udp: 4096
;; QUESTION SECTION:
;example.com.         IN A

;; ANSWER SECTION:
example.com.          86400 IN A 93.184.216.119
example.com.          86400 IN RRSIG A 7 2 86400 20131227171405 (
                            20131127171405 40400 example.com.
                            JCoL8L7As1a8CXnx1W62O94eQl6zvVQ3prtNK7BWIW9O
                            lir/4V+a6c+0tbt4z4lhgmb0sb+qdvqRnlI7CydaSZDb
                            hlrJA93fHqFqNXw084YD1gWC+M8m3ewbobiZgBUh5W66
                            1hsVjWZGvvQL+HmobuSvsF8WBMAFgJgYLg0YzBAvwHIk
                            886be6vbNeAltvPl9I+tjllXkMK5dReMH40ulgKo+Cwb
                            xNQ+RfHhCQIwKgyvL1JGuHB125rdEQEVnMy26bDcC9R+
                            qJNYj751CEUZxEEGI9cZkD44oHwDvPgF16hpNZGUdo8P
                            GtuH4JwP3hDIpNtGTsQrFWYWL5pUuuQRwA== )

;; AUTHORITY SECTION:
example.com.          86400 IN NS master.example.com.
example.com.          86400 IN NS slave.example.com.
example.com.          86400 IN RRSIG NS 7 2 86400 20131227171405 (
                            20131127171405 40400 example.com.
                            hEGzNvKnc3sXkiQKo9/+ylU5WSFWudbUc3PAZvFMjyRA
                            j7dzcVwM5oArK5eXJ8/77CxL3rfwGvi4LJzPQjw2xvDI
                            oVKei2GJNYekU38XUwzSMrA9hnkremX/KoT4Wd0K1NPy
                            giaBgyyGR+PT3jIP95Ud6J0YS3+zg60Zmr9iQPBifH3p
                            QrvvY3OjXWYL1FKBK9+rJcwzlsSslbmj8ndL1OBKPEX3
                            psSwneMAE4PqSgbcWtGlzySdmJLKqbI1oB+d3I3bVWRJ
                            4F6CpIRRCb53pqLvxWQw/NXyVefNTX8CwOb/uanCCMH8
                            wTYkCS3APl/hu20Y4R5f6xyt8JZx3zkZEQ== )

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Thu Nov 28 00:01:06 2013
;; MSG SIZE  rcvd: 1335
```

Si le DNSSEC est fonctionnel, les RRSIG record devraient être présent dans la section `ANSWER`

Option 2: Utiliser un site d'analyse DNS tel que [celui ci](https://dnssec-analyzer.verisignlabs.com):

![image](https://github.com/thePFLy/spotifyDisplayer/assets/92318644/3b07e69b-1475-42a4-a4d0-62e2d7dc37dd)

Si le DNSSEC est fonctionnel, l'analyse du site devrait afficher des RRSIG et DNSKEY validés.

## Bibliographie
1. Ahona Rudra, [Délégation de sous-domaines](https://powerdmarc.com/fr/dmarc-subdomain-delegation/), 5 aout 2022, consulté le 23/05/2023
2. Michael Buckbee, [Qu’est-ce que le DNS, comment fonctionne-t-il et quelles sont ses vulnérabilités ?](https://www.varonis.com/fr/blog/dns-kezako), 7 mars 2019, consulté le 23/05/2023
3. nameShield, [DNS Cache Poisoning](https://www.nameshield.com/ressources/lexique/dns-cache-poisoning/), consulté le 24/05/2023
4. Wikipedia, [Empoisonnement du cache DNS](https://fr.wikipedia.org/wiki/Empoisonnement_du_cache_DNS), 18 janvier 2023, consulté le 24/05/2023
5. Praveen N, [SOA Servers Configuration and DMZ ](http://praveensreenivas.blogspot.com/2016/03/dmz-computing.html), 8 mars 2016, consulté le 23/05/2023
6. Ahona Rudra ,[Types de DNS : Explication des types de requêtes, serveurs et enregistrements DNS](https://powerdmarc.com/fr/dns-types-queries-and-servers/amp/), il y'a 5 mois, consulté le 23/05/2023
7. One ,[What is a registry, registrar and registrant?](https://help.one.com/hc/en-us/articles/115005588149-What-is-a-registry-registrar-and-registrant-), consulté le 23/05/2023
8. Perrin Varin ,[Comprendre la hiérarchie des institutions Registre, Bureau d’Enregistrement (Registrar), Revendeur (Reseller) et Registrant. ](https://blog.netim.com/fr/news-netim/comprendre-la-hierarchie-des-institutions-registre-bureau-denregistrement-registrar-revendeur-reseller-et-registrant-1414/), 16 juillet 2019, consulté le 23/05/2023
9. Mathieu ,[La délégation NS d'un sous-domaine, cet illustre inconnu](https://uname.pingveno.net/blog/index.php/post/2019/03/02/La-d%C3%A9l%C3%A9gation-NS-d-un-sous-domaine%2C-cet-illustre-inconnu), 5 mars 2019, consulté le 23/05/2023
10. Romain Guichard, [Délégation d'une zone DNS](http://blog.vsense.fr/delegation-dune-zone-dns.html), 4 mars 2013, consulté le 23/05/2023
11. Kaspersky, [Qu’est-ce que le pharming et comment l’éviter ?](https://www.kaspersky.fr/resource-center/definitions/pharming), consulté le 24/05/2023
12. Wikipedia ,[Pharming](https://fr.wikipedia.org/wiki/Pharming), 1 mars 2023,consulté le 24/05/2023
13. Inet, [Le DNS ID spoofing](https://www.inetdoc.net/guides/tutoriel-secu/tutoriel.securite.attaquesprotocoles.dns.html), , consulté le 24/05/2023
14. Erez Hasson ,[DNS Spoofing](https://www.imperva.com/learn/application-security/dns-spoofing/), 2 mai 2023, consulté le 24/05/2023
15. Wikipedia ,[DNS spoofing](https://en.wikipedia.org/wiki/DNS_spoofing), 13 avril 2023, consulté le 24/05/2023
16. Michael Buckbee, [Qu’est-ce que le DNS Tunneling ? Guide de détection](https://www.varonis.com/fr/blog/quest-ce-que-le-dns-tunneling-guide-de-detection), 6 avril 2023, consulté le 24/05/2023
17. Jaromil ,[Qu’est-ce que le DNS Tunneling ? Guide Complet](https://www.ambient-it.net/dns-tunneling-implementation/), 11 janvier 2023, consulté le 24/05/2023
18. Icann, [DNSSEC – Qu'est-ce que c'est et pourquoi est-ce important ?](https://www.icann.org/resources/pages/dnssec-what-is-it-why-important-2019-03-20-fr), consulté le 23/05/2023
19. Wikipedia, [Domain Name System Security Extensions](https://fr.wikipedia.org/wiki/Domain_Name_System_Security_Extensions), 8 juillet 2022, consulté le 23/05/2023
20. CloudFlare ,[Comment fonctionne DNSSEC](https://www.cloudflare.com/fr-fr/dns/dnssec/how-dnssec-works/), consulté le 24/05/2023
21. Jesin A,[How To Setup DNSSEC on an Authoritative BIND DNS Server](https://www.digitalocean.com/community/tutorials/how-to-setup-dnssec-on-an-authoritative-bind-dns-server-2), 19 mars 2014, consulté le 24/05/2023
22. Klinsmann Öteyo,[How To Secure BIND DNS Server With DNSSec Keys](https://computingforgeeks.com/secure-bind-dns-server-with-dnssec-keys/), 22 octobre 2022, consulté le 24/05/2023
23. Computer Security Resource Center ,[Zone Signing Key (ZSK)](https://csrc.nist.gov/glossary/term/zone_signing_key), consulté le 24/02/2023