[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# Wireshark

## Wireshark ? c'est quoi ?[^1] [^2] [^3] [^4]

Wireshark est un analyseur de paquets réseau créer en 1998 par Gerald Combs. C'est un logiciel qui permet d'analyser un réseau et de récupérer tous les paquets (un "paquet" est un message d’un des nombreux protocoles réseau) y passant pour ensuite le décomposer et le traduire en données interprétable par des humains. 
Wireshark est utilisé notamment pour :  
- L'analyse et le troubleshooting réseau.
- La sécurité réseau.
- Le reverse engineering.
- Le développement et le début de protocoles.
- L’éducation.  

Aujourd’hui que ce soient des agences gouvernementales, de grandes entreprises ou bien des écoles, tout le monde utilise Wireshark. Car bien qu’il existe des alternatives sur le marché (comme tcpdump ou cloudShark qui sont les principaux concurrents) c’est bien Wireshark qui est le plus populaire et ce grâce à ces filtre très nombreux, ces fonctionnalités variés et le nombres de protocoles que ce dernier peut interpréter (actuellement 1515 selon Wikipédia[^2]).
Wireshark est disponible sur tous les OS et une version en ligne de commande, nommé Tshark, existe aussi.

## Fonctionalités: [^1] [^2] [^3] [^5]

Voici une liste de fonctionnalités non exhaustif de Wireshark mais suffisante pour commencer.

### La capture de paquets:

La fonctionalisé principal et de base de WireShark est la capture de paquets. Pour commencer une capture ovrez Wireshark et sélectionner un réseau que vous voulez observer en double clicant dessus.  
![image](https://user-images.githubusercontent.com/62069633/170109781-124844a2-4c1d-4583-8e6c-85e1d788fb4e.png)
Lorsque vous souhaiter arrêter la capture appuyer sur le petit carréer rouge en haut à gauche.  
Vous pouvez znsuite travailler dessus ou enregistrer la capture pour revenir dessus plus tard en allant dant **fichier -> enregistrer sous**.  
Pour recommencer une capture clicker sur l'aileron bleu, à gauche du carré rouge.
![image](https://user-images.githubusercontent.com/62069633/170111507-3912c10a-ce96-4149-b715-db2f774c736e.png)

### L'analyse de paquets

Maintenant que nous avons une capture Wireshark nous permet de l'analyser. Pour ce faire nous auron l'aide de trois fenêtres:  
1. Le packet list
2. Le packet details
3. Le packet bytes pane  

![image](https://user-images.githubusercontent.com/62069633/170119431-f55549da-1adb-4bfb-8e43-2ae49e237546.png)

### 1. Le packet list

Le packet list nous permets de voir tous les paquets enregistrer dans cette capture. Cette fenêtre vous donnent déjà des informations sur général classés dans différentes colonnes. Vous pouvez si vous le souhaitez suprimer, ajouter ou modifier des colonnes en faisant un clic droit sur l'entête.  
Cependant les colonnes par défault seront généralement emplement suffisante. 
- No. : cette colonne numérote les paquets capturés et indique à quel conversation aprtients le paquet: si en clicant sur un paquet vous voyez qu'un ligne pleines apprait sur un autr paquets c'est qu'ils sont dans une même conversation (par exemple three handshake).
![image](https://user-images.githubusercontent.com/62069633/170123265-fcdcf935-272d-42d4-9b04-e25624661fe5.png)
Les paquets qui n'ont pas de lignes mes des poitillés ne font pas parti de cette conversation.
![image](https://user-images.githubusercontent.com/62069633/170123621-fe1cc571-e45b-43c0-bf79-532116f7d749.png)
- Time : cette colonne indique quand sont arrivé les paquets après le début de la capture.
- Source : indique l'adresse IP qui à envoyé le paquet.
- Destination : indique l'adresse IP qui à ressus le paquet.
- Protocole : indique le protocole utilisé.
- Length : indique la taille du paquet (en octets).
- Info : cette colonne donne des informations différente en fonction du protocole utilisé.

### 2. Le packet details

Lorsque vous sélectionner un packet dans Le packet list vous pourrez le retrouvez tous les protocoles utilisé par ce paquet dans cette fenêtre.
Vous pouvez dévlopez un protocol en faisant un clic gauche dessus.  
De plus vous pouvez faire ne faire appraitre dans le packet list que les paquets utilisant le même protocoles en faisant un click droit sur le protocole **Appliquer comme un filtre -> Sélectionné** ou n'afficher que les paquets de la conversation avec **Filtre de conversation -> le protocole souhaiter**.

### 3. Le packet bytes pane

Ici vous retrouverez le paquet brut au format hexadécimale ou binaire (click droit -> ... comme bits) avec une traduction au format ASCII. Si vous cliquez sur un bit celà sélectionnera dans le Le packet details à quoi il correspond (et vice versa).



## Bibliographie

[^1]:* [Wireshark User’s Guide](https://www.wireshark.org/docs/wsug_html_chunked/), auteur inconnu, date de création inconnu, consulté le (24/05/2022)  
   Résumé : documentation officiel de Wireshark  
   Avis sur la ressource : clair et très complet malgré un design un peu rebutant  
   
[^2]:* [Wireshark](https://fr.wikipedia.org/wiki/Wireshark), wikipedia, consulté le (24/05/2022)  
Résumé : page wikepedia Wireshark  
Avis sur la ressource : très peu complète mais donnes quelques information de bases  
   
[^3]:* [Comment utiliser Wireshark: tutoriel complet + astuces](https://www.varonis.com/fr/blog/comment-utiliser-wireshark), Jeff Petters, 12 mai 2021, consulté le (24/05/2022)  
Résumé : petit guide concis sur wireshark  
Avis sur la ressource : petit guide concis mais clair et agréable à lire  

[^4]:* [Network traffic analysis for IR: Alternatives to Wireshark](https://resources.infosecinstitute.com/topic/network-traffic-analysis-for-ir-alternatives-to-wireshark/), Patrick Mallory, 12 novembre 2019, consulté le (24/05/2022)  
Résumé : alternatives à Wireshark
Avis sur la ressource : présente bien les alternatives

[^5]:* [How to Use Wireshark: A Complete Tutorial](https://www.lifewire.com/wireshark-tutorial-4143298),  Scott Orgera, 8 juillet 2020, consulté le (24/05/2022)  
Résumé : autre guide sur wireshark  
Avis sur la ressource : très clair mais trop court
