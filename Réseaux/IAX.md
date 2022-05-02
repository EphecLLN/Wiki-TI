# Inter-Asterisk eXchange

IAX est un protocole VoIp produit et créé à la base pour le logiciel PBX « Asterisk ».

## VoIp et Asterisk :

IAX fait donc partie intégrante du système VoIp mis en place par le PBX Asterisk, mais qu’est-ce qu’Asterisk et la VoIp ?

### >Qu’est-ce qu’Asterisk ?:

Asterisk est un logiciel PABX (Autocommutateur téléphonique privé) créé en 1999 par Mark Spencer. Ce logiciel est très utilisé dans les entreprises ou les call-centers et est à la base de nombreux autres logiciels de communication. Du à sa popularité, il possède une grosse communauté . Il utilise la VoIp pour transmettre toutes sortes de médias (voix, vidéo, images, etc).

### >Qu’est-ce que la VoIp ?:

VoIp sont les initiales de « Voice over IP », et comme son nom l’indique, c’est de la téléphonie (transmission de voix ) en utilisant le protocole IP et donc via internet .
La VoIp offre de nombreux avantages comparé à la téléphonie classique. En effet, le fait de ne plus avoir une ligne pour les données et une ligne pour la transmission des appels est assez révolutionnaire. Cette technologie permet donc de centraliser le tous en utilisant le protocole IP . La VoIp devient également un standard dans les entreprises dû à son cout réduit en installation et en entretient.
Principales caractéristiques d’IAX :
IAX est utilisé en téléphonie sur IP, mais qu’elles sont ses caractéristiques et en quoi est-t-il différent des autres protocoles VoIp ?

## Avantages comparé à la VoIp classique :

-   **Ports** : En VoIp classique, on utilise deux ports, le port 4005 pour transmettre le flux de données et le port 4060 pour transmettre la signalisation relative à ce flux. Cependant l’avantage de IAX est qu’il utilise un seul port, le 4569, à la fois pour la signalisation et le flux de données. Le fait d’exposé une seul port présentes divers avantages. En effet, la configuration du firewall est plus simple et car moins de ports sont exposés la NAT est donc également rendue plus simple à configurée. Cette caractéristique apporte donc une certaine « légèreté » en terme de configuration pour ce protocole.
-   **Diminution de la bande passante** : Sa première caractéristique (unique port) apporte un avantage sur l’utilisation des ressources. Effectivement, cela inclus une réduction de la bande passante utilisée

-   **Diminution de la latence** : Le faite que un seul port soit utilisé au lieux d’un seul, apporte aussi un changement au niveau de la latence. En effet, comme les deux flux traditionnels sont envoyé en même temps et sur le même ports, on a une réduction de la latence
-   **Rapidité** :Ces trois premiers avantages apportes une vitesse pour la communication qui est plus importante qu’avec SIP.

## Défauts de IAX :

Malgré ces avantages, IAX n’est pas autant utilisé que SIP, qui est le protocole de base de la VoIp . Nous allons donc voir quels sont les défauts d’IAX et qui seraient la raison pour laquelle SIP est toujours une base solide pour faire de la VoIp.

-   **Pas de 3-way-handshake** , c’est-à-dire qu’il n’y pas d’établissement de la connexion par les 3 étapes classiques. Cette caractéristique rend le système sensible au DOS. En effet,si on ne dispose pas d’établissement de connexion via 3-way handshake il y a possibilité d’attaque Dos sur des ressources publiques (attaque par dénis de service). La meilleur façon de contrer ce problème est de créer une « white-list » avec toutes les adresses de confiance.
-   **Construit sur base d’un système Asterisk** : Bien qu’une portabilité est envisageable vers d’autres systèmes que celui d’Asterisk, le fait que IAX est été créé à la base pour ce dernier apporte un problème. En fait, il est impossible d’utiliser IAX sans un router typé Asterisk . Cela pointe l’un des plus gros défauts de IAX, il n’est pas très polyvalent et adaptatif, là où des protocoles comme SIP le sont beaucoup plus. La dépendance à Asterisk est aussi ce qui rend ce protocole moins flexible que SIP ou MGCP, car le protocole n’a pas été conçu dans ce but-là.

**Bibliographie:**

https://speetis.fei.tuke.sk/KomunikacnaTechnika1/prednasky/26_9_2016/ST_ASTERISK_3_12_2013/Asterisk/Inter-Asterisk%20Exchange%20(IAX).pdf

https://en.wikipedia.org/wiki/Inter-Asterisk_eXchange

https://www.techtarget.com/searchunifiedcommunications/definition/IAX-Inter-Asterisk-Exchange-Protocol#:~:text=IAX%20is%20well%20suited%20for,of%20multiplexing%20and%20trunking.

https://askanydifference.com/difference-between-sip-and-iax/

https://www.voip-info.org/iax-versus-sip/#:~:text=1)
