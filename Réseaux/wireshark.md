---
layout: default
title: Wireshark
parent: Réseaux
---

# Wireshark

## Wireshark ? C'est quoi ? [^2] [^4]

Wireshark est un analyseur de paquets réseau créé en 1998 par Gerald Combs. C'est un logiciel qui permet d'analyser un réseau et de récupérer tous les paquets (un "paquet" est un message d’un des nombreux protocoles réseau) y passant pour ensuite le décomposer et le traduire en données interprétables par des humains.
Wireshark est utilisé notamment pour :

- L'analyse et le troubleshooting réseau.
- La sécurité réseau.
- Le reverse engineering.
- Le développement et le début de protocoles.
- L’éducation.

Aujourd’hui que ce soient des agences gouvernementales, de grandes entreprises ou bien des écoles, tout le monde utilise Wireshark. Car bien qu’il existe des alternatives sur le marché (comme tcpdump ou cloudShark qui sont les principaux concurrents) c’est bien Wireshark qui est le plus populaire et ce grâce à ses très nombreux filtres , ses fonctionnalités variées et le nombres de protocoles que ce dernier peut interpréter (actuellement 1515 selon Wikipédia [^2]).
Wireshark est disponible sur tous les OS et une version en ligne de commande, nommé Tshark, existe aussi.

## Fonctionnalités : [^2] [^5]

Voici une liste de fonctionnalités non exhaustive de Wireshark mais suffisante pour commencer.

### La capture de paquets :

La fonctionnalité principale et de base de Wireshark est la capture de paquets. Pour commencer une capture, ouvrez Wireshark et sélectionnez un réseau que vous voulez observer en double cliquant dessus.
![image](https://user-images.githubusercontent.com/62069633/170109781-124844a2-4c1d-4583-8e6c-85e1d788fb4e.png)
Lorsque vous souhaitez arrêter la capture, appuyez sur le petit carré rouge en haut à gauche.
Vous pouvez ensuite travailler dessus ou enregistrer la capture pour revenir dessus plus tard en allant dans **fichier -> enregistrer sous**.
Pour recommencer une capture, cliquez sur l'aileron bleu, à gauche du carré rouge.
![image](https://user-images.githubusercontent.com/62069633/170111507-3912c10a-ce96-4149-b715-db2f774c736e.png)

### L'analyse de paquets

Maintenant que nous avons une capture Wireshark, cela nous permet de l'analyser. Pour ce faire nous aurons l'aide de trois fenêtres :

1. Le packet list
2. Le packet details
3. Le packet bytes pane

![image](https://user-images.githubusercontent.com/62069633/170119431-f55549da-1adb-4bfb-8e43-2ae49e237546.png)

#### 1. Le packet list

Le packet list nous permet de voir tous les paquets enregistrés dans cette capture. Cette fenêtre vous communique déjà des informations générales classées dans différentes colonnes. Vous pouvez si vous le souhaitez supprimer, ajouter ou modifier des colonnes en faisant un clic droit sur l'entête.Cependant les colonnes par défaut seront généralement amplement suffisantes.

- No. : cette colonne numérote les paquets capturés et indique à quelle conversation appartient le paquet : si en cliquant sur un paquet vous voyez qu'une ligne pleine apparait sur un autre paquet, c'est qu'ils sont dans une même conversation (par exemple three handshake).![image](https://user-images.githubusercontent.com/62069633/170123265-fcdcf935-272d-42d4-9b04-e25624661fe5.png)Les paquets qui n'ont pas de lignes mais des pointillés ne font pas partie de cette conversation.![image](https://user-images.githubusercontent.com/62069633/170123621-fe1cc571-e45b-43c0-bf79-532116f7d749.png)
- Time : cette colonne indique quand sont arrivés les paquets après le début de la capture.
- Source : indique l'adresse IP qui a envoyé le paquet.
- Destination : indique l'adresse IP qui a reçu le paquet.
- Protocole : indique le protocole utilisé.
- Length : indique la taille du paquet (en octets).
- Info : cette colonne donne des informations différentes en fonction du protocole utilisé.

#### 2. Le packet details

Lorsque vous sélectionnez un packet dans le packet list, vous pourrez retrouver tous les protocoles utilisés par ce paquet dans cette fenêtre.
Vous pouvez développer un protocole en faisant un clic gauche dessus.
De plus, vous pouvez, dans le packet list, ne faire apparaitre que les paquets utilisant le même protocole en faisant un clic droit sur le protocole **Appliquer comme un filtre -> Sélectionné** ou n'afficher que les paquets de la conversation avec **Filtre de conversation -> le protocole souhaité**.

#### 3. Le packet bytes pane

Ici, vous retrouverez le paquet brut au format hexadécimal ou binaire (click droit -> ... comme bits) avec une traduction au format ASCII. Si vous cliquez sur un bit cela sélectionnera dans le packet détails ce à quoi il correspond (et vice versa).

### Les filtres : [^7]

Il existe deux types de filtre dans Wireshark : les filtres de capture et les filtres d'affichage. Le premier se met avant une capture et permet de ne capturer que ce qui nous intéresse et de réduire la taille du fichier de capture. Le second lui s'applique après une capture et ne fait que cacher les paquets qui ne nous intéressent pas.

#### Filtres de capture :

Pour appliquer un filtre de capture, entrez, avant la capture, le filtre ici :
![image](https://user-images.githubusercontent.com/62069633/170136920-c9313d66-dd70-4da5-9b51-57b10902a2ca.png)

Si vous souhaitez entrer deux filtres, utilisez le mot clé : `and`
Si vous souhaitez appliquer un filtre inversé (donc : capturer tous sauf), utilisez le mot clé: `not`

Wireshark utilise des filtres par défaut s’il est utilisé à distance pour ne pas enregistrer ce qui est lié à cette connexion (comme ssh).

##### Exemples utiles (trouver ici [^6])

Pour capturer tout se qui vient et sort d'une adresse IP :
`host 172.18.5.4`
Pour capturer tout ce qui concerne un range d’IP :
`net 192.168.0.0/24`
Pour capturer tout ce qui concerne un port :
`port 53`
Pour ne capturer que du trafic IPv4 :
`ip`
Pour ne capturer que du trafic unicast :
`not broadcast and not multicast`

#### Filtres d’affichage :

Pour appliquer un filtre de capture, entrez, le filtre ici :
![image](https://user-images.githubusercontent.com/62069633/170139504-3bd37f17-fa69-4376-b305-01c3f09a3876.png)

Si vous souhaitez n'afficher que les paquets qui correspondent à deux filtres, utiliser le mot clé : `and` ou `&&`
Si vous souhaitez n'afficher que les paquets qui correspondent à deux filtres, utiliser le mot clé : `or` ou `||`

##### Exemples utiles (trouvés ici [^7])

Pour n'afficher que les paquets possédant un protocole :
`TCP`
Pour n'afficher que ce qui correspond à une adresse IP :
`ip.addr==172.18.5.4`
Pour capturer tout ce qui concerne un range d’IP :
`ip.addr==192.168.0.0/24`
Pour capturer tout ce qui concerne un port :
`tcp.port==53`
Pour ne capturer que le trafic entre deux machines (par exemple client et serveur):
`ip.src==192.168.18.5 and ip.dst==192.168.2.45`

### La colorisation : [^8]

La colorisation permet de mettre en évidence des paquets qui répondent à certains filtres sans pour autant cacher les autres. Wireshark propose, de base, 20 filtres. Pour ajouter ou supprimer des filtres aller dans **vue -> coloring rules**.
![image](https://user-images.githubusercontent.com/62069633/170142753-0138ffd1-61dd-4164-a87f-9ad87fc1daee.png)

Vous pouvez ainsi remarquer qu'il est possible d'importer ou d'exporter des règles de colorisation. Ainsi vous pouvez avoir plusieurs sets de règles que vous utiliserez dans différentes utilisations et vous pourrez aller chercher des règles préfaites sur internet pour des cas spécifiques.

## Bibliographie

[^1]: [Wireshark User’s Guide](https://www.wireshark.org/docs/wsug_html_chunked/), auteur inconnu, date de création inconnu, consulté le (24/05/2022)
         Résumé : documentation officielle de Wireshark
         Avis sur la ressource : clair et très complet malgré un design un peu rebutant
    
[^2]: [Wireshark](https://fr.wikipedia.org/wiki/Wireshark), wikipedia, consulté le (24/05/2022)
         Résumé : page Wikipédia Wireshark
         Avis sur la ressource : très peu complète mais donnes quelques informations de bases
    
[^3]: [Comment utiliser Wireshark: tutoriel complet + astuces](https://www.varonis.com/fr/blog/comment-utiliser-wireshark), Jeff Petters, 12 mai 2021, consulté le (24/05/2022)
         Résumé : petit guide concis sur wireshark
         Avis sur la ressource : petit guide concis mais clair et agréable à lire
    
[^4]: [Network traffic analysis for IR: Alternatives to Wireshark](https://resources.infosecinstitute.com/topic/network-traffic-analysis-for-ir-alternatives-to-wireshark/), Patrick Mallory, 12 novembre 2019, consulté le (24/05/2022)
         Résumé : alternatives à Wireshark
         Avis sur la ressource : présente bien les alternatives
    
[^5]: [How to Use Wireshark: A Complete Tutorial](https://www.lifewire.com/wireshark-tutorial-4143298),  Scott Orgera, 8 juillet 2020, consulté le (24/05/2022)
         Résumé : autre guide sur wireshark
         Avis sur la ressource : très clair mais trop court
    
[^6]: [CaptureFilters](https://wiki.wireshark.org/CaptureFilters),  auteur inconnu, date de création inconnu, consulté le (24/05/2022)
         Résumé : guide sur les filtres de captures
         Avis sur la ressource : concis et donne des exemples utiles
    
[^7]: [DisplayFilters](https://wiki.wireshark.org/DisplayFilters),  auteur inconnu, date de création inconnu, consulté le (24/05/2022)
         Résumé : guide sur les filtres d'affichage
         Avis sur la ressource : concis et donne des exemples utiles
[^8]: [ColoringRules](https://wiki.wireshark.org/ColoringRules),  auteur inconnu, date de création inconnu, consulté le (24/05/2022)
      Résumé : guide sur les la coloration
      Avis sur la ressource : concis et donne des exemples utiles
