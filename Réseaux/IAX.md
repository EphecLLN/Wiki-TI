# Inter-Asterisk eXchange

IAX est un protocole VoIP produit et créé à la base pour le logiciel PBX « Asterisk ». Il est utilisé pour transmettre des sessions VoIP entre les serveurs et les machines des clients.

## VoIP et Asterisk :

IAX fait donc partie intégrante du système VoIP mis en place par le PBX Asterisk, mais qu’est-ce qu’Asterisk et la VoIP ?

### > Qu’est-ce qu’Asterisk ?:

![image](https://user-images.githubusercontent.com/71372371/166737739-e9360031-97ac-4d70-8657-8113ab4634f0.png)

Asterisk est un logiciel PABX (Autocommutateur téléphonique privé) créé en 1999 par Mark Spencer.

Ce logiciel est très utilisé dans les entreprises ou les calls-centers et est à la base de nombreux autres logiciels de communication. Dû à sa popularité, il possède une grosse communauté. Il utilise le système VoIP pour transmettre toutes sortes de médias (voix, vidéo, images, etc) [[8](https://en.wikipedia.org/wiki/Asterisk)].

### > Qu’est-ce que la VoIP ?:

VoIP sont les initiales de « Voice over IP », et comme son nom l’indique, c’est de la téléphonie (transmission de voix) en utilisant le protocole IP et donc via internet.

La VoIP offre de nombreux avantages comparés à la téléphonie classique. En effet, le fait de ne plus avoir une ligne pour les données et une ligne pour la transmission des appels est assez révolutionnaire. Cette technologie permet donc de transmettre le tout (voix et données) sur le même type de support. La VoIP devient également un standard dans les entreprises dû à son cout réduit pour son installation et son entretien.

## Principales caractéristiques d’IAX :

IAX est utilisé en téléphonie sur IP, mais quelles sont ses caractéristiques et en quoi est-il différent des autres protocoles VoIP ?

### > Avantages comparé à la VoIP classique :

-   **Ports** : En VoIP classique, on utilise deux ports, le port 4005 pour transmettre le flux de données et le port 4060 pour transmettre la signalisation relative à ce flux. Cependant l’avantage de IAX est qu’il utilise un seul port, le 4569 en UDP, à la fois pour la signalisation et le flux de données [[2](https://en.wikipedia.org/wiki/Inter-Asterisk_eXchange), [3](https://www.voip-info.org/iax-versus-sip/.)]. Le fait d’exposer un seul port présente divers avantages. En effet, la configuration du firewall est plus simple et la NAT est également rendue plus simple à configurer. Cette caractéristique apporte donc une certaine « légèreté » en matière de configuration pour ce protocole [[4](https://askanydifference.com/difference-between-sip-and-iax/)].

-   **Diminution de la bande passante** : Sa première caractéristique (unique port) apporte un avantage sur l’utilisation des ressources. De fil en aiguille, cela inclut une réduction de la bande passante utilisée.

-   **Diminution de la latence** : Le fait qu'un seul port soit utilisé au lieu d’un seul, apporte aussi un changement au niveau de la latence. En effet, comme les deux flux traditionnels sont envoyés en même temps et sur le même port, on a une réduction de la latence

-   **Rapidité** : Ces trois premiers avantages apportent donc une vitesse pour la communication qui est plus importante qu’avec d'autres protocoles comme SIP par exemple.

-   **Protocole binaire** :
    Le fait que AIX soit un [**protocole binaire**](https://github.com/Pourbaix/Wiki-TI/blob/main/R%C3%A9seaux/IAX.md#concepts) apporte un grand plus point de vue sécurité, en effet il est plus difficile de trouver des failles. SIP est quant à lui un exemple de protocole basé sur du texte [[5](https://www.voip-info.org/iax-versus-sip/#:~:text=1)].

### > Défauts de IAX :

Malgré ces avantages, IAX n’est pas autant utilisé que SIP, qui est le protocole le plus utilisé pour la VoIP.

Nous allons donc voir quels sont les défauts d’IAX qui seraient la raison pour laquelle SIP est toujours une base solide pour faire de la VoIP.

-   **Construit sur base d’un système Asterisk** : Bien qu’une portabilité soit envisageable vers d’autres systèmes que celui d’Asterisk, le fait que IAX a été créé à la base pour ce dernier apporte un problème. En fait, il est impossible d’utiliser IAX sans un router typé Asterisk . Cela pointe l’un des plus gros défauts de IAX, il n’est pas très polyvalent et adaptatif, là où des protocoles comme SIP le sont beaucoup plus. La dépendance à Asterisk est aussi ce qui rend ce protocole moins flexible que SIP ou MGCP, car le protocole n’a pas été conçu dans le même but.

-   **Sensible au déni de service** : L'une des forces d'IAX est aussi une de ses faiblesses. En effet, comme ce protocole n'utilise qu'un port qui est bien connu, il est sensible aux [**attaques par déni de service**](https://github.com/Pourbaix/Wiki-TI/blob/main/R%C3%A9seaux/IAX.md#concepts).

## Caractéristiques techniques:

### > Protocoles:

Au lieux d'utiliser RTP, IAX utilise UDP sur un seul port (le 4569) comme précisé précédemment.
Toute la partie signalisation est gérée en couche 2 (Liaison de données).

### > Structure des données:

IAX envoit des paquets avec la structure suivante:

-   20 octets pour l'entête IP (**=** autres protocoles),
-   8 octets pour l'entête UDP (**=** autres protocoles),
-   4 octets pour le "**_mini-entête_**" de IAX ( au lieux de 12 octets pour les autres protocoles),
-   Le payload dont la taille varie selon le [**codec**](https://github.com/Pourbaix/Wiki-TI/blob/main/R%C3%A9seaux/IAX.md#concepts).

![image](https://user-images.githubusercontent.com/71372371/167297794-8ca673cb-c247-4b8d-8fe3-4b2b8467bb9e.png)


[SOURCE](https://www.semanticscholar.org/paper/A-Comparative-Study-between-Inter-Asterisk-Exchange-Aliwi-Sumari/58eb892d9574cd17312ce98adc9e7009695e3094/figure/3)

C'est cette structure qui diminue l'utilisation de la bande passante. Cette "mini-entête" IAX apporte un gros plus car moins de données sont échangées et donc moins de bande passante est utilisée.

### > Échange type:

Voici un exemple d'échange type lors d'une communication avec le protocole IAX:

### **Explications:**

## **Concepts**:

### C01 - Attaque par déni de service:

Une attaque par déni de service (appellé DoS en anglais) consiste à rendre indisponible un service et donc d'empêcher les utilisateurs de l'utiliser. De manière générale, cette attaque consiste à surcharger un réseau ou un serveur en s'y connectant et en envoyant des requêtes avec une quantité importante d'appareils.[[7](https://fr.wikipedia.org/wiki/Attaque_par_déni_de_service)]

### C02 - Protocoles binaires:

Il existe deux types de protocoles; des protocoles binaires où les informations sont transmises sous une forme où le protocole utilise toutes les valeurs d'un octet. Et d'un autre coté, les protocoles dit basé sur du texte, transmettent eux des valeurs qui correspondent à des caractères lisibles par l'être humain (par exemple de l'ASCII) [[6](https://en.wikipedia.org/wiki/Communication_protocol#:~:text=A%20binary%20protocol%20utilizes%20all,rather%20than%20a%20human%20being)].

### C03 - Codecs:

Les codecs sont des manières d'encoder l'information transmise qui détermines la qualité audio, la bande-passante, la compression de la voix, etc.

Il sont généralement caractérisé par 3 choses:

-   Le taux d'échantillonnage
-   Le débit binaire
-   La bande-passante utilisée

En VoIP il en existe plusieurs, les 3 plus connus sont:

1. **G.711**, inventé en 1972, il propose une qualité sonore élevée mais requière une assez grosse bande-passante.

2. **G.722HD**, c'est un codec dit Haute-Définition (HD) qui fut créé en 1988.

3. **G.729**, c'est l'un des codecs les plus légé en bande-passante et avec une qualité audio résonable.

Source => [9](https://www.nextiva.com/blog/voip-codecs.html#:~:text=A%20VoIP%20codec%20is%20a,two%20terms%3A%20Compression%20and%20Decompression.)

## **Bibliographie:**

### - [1](<https://speetis.fei.tuke.sk/KomunikacnaTechnika1/prednasky/26_9_2016/ST_ASTERISK_3_12_2013/Asterisk/Inter-Asterisk%20Exchange%20(IAX).pdf>)

Titre: Inter-Asterisk eXchange: Deployement scenariosin SIP-enabled nteworks

Auteur: WILEY, John Wiley & Sons

Date de parution: 2009

Date de visite: 29/04/2022

### - [2](https://en.wikipedia.org/wiki/Inter-Asterisk_eXchange)

Titre: Inter-Asterisk eXchange

Auteur: ?

Date de parution: 7 Novembre 2021

Date de visite: 27/04/2022

### - [3](https://www.voip-info.org/iax-versus-sip/.)

Titre: IAX (Inter-Asterisk Exchange Protocol)

Auteur: Mark Spencer

Date de parution: 1 Juin 2005

Date de visite: 23/04/2022

### - [4](https://askanydifference.com/difference-between-sip-and-iax/)

Titre: Difference Between SIP and IAX (With Table)

Auteur: askanydifference

Date de parution: ?

Date de visite: 28/04/2022

### - [5](https://www.voip-info.org/iax-versus-sip/#:~:text=1)

Titre: IAX versus SIP

Auteur: VoIP Info

Date de parution: 1 juin 2005

Date de visite: 23/04/2022

### - [6](https://en.wikipedia.org/wiki/Communication_protocol#:~:text=A%20binary%20protocol%20utilizes%20all,rather%20than%20a%20human%20being.)

Titre: Communication protocol

Auteur: Wikipedia

Date de parution: 26 Avril 202

Date de visite: 1/05/2022

### - [7](https://fr.wikipedia.org/wiki/Attaque_par_déni_de_service)

Titre: Attaque par déni de service

Auteur: Wikipedia

Date de parution: 1 avril 2022

Date de visite: 1/05/2022

### - [8](https://en.wikipedia.org/wiki/Asterisk)

Titre: Asterisk

Auteur: Wikipedia

Date de parution: 4 mai 2022

Date de visite: 25/04/2022

### - [9](https://www.nextiva.com/blog/voip-codecs.html#:~:text=A%20VoIP%20codec%20is%20a,two%20terms%3A%20Compression%20and%20Decompression.)

Titre: What Are VoIP Codecs & How Do They Affect Call Sound Quality?

Auteur: JEREMIAH ZERBY

Date de parution: 16 novembre 2020

Date de visite: 08/05/2022
