***
# Synthèse sur DKIM

## Qu'est-ce que DKIM et à quoi ça sert ?

DKIM (DomainKeys Identified Mail) est une méthode d'authentification utilisée pour empêcher la falsification des adresses électroniques et renforcer la sécurité des courriers électroniques.
Elle offre un moyen fiable pour le serveur de messagerie du destinataire de vérifier l'authenticité du domaine de l'expéditeur. L'un des objectifs principaux de DKIM est de protéger 
l'expéditeur contre les risques liés au spam en démontrant aux filtres anti-spam qu'il n'est pas un spammeur malveillant. En utilisant DKIM, les expéditeurs légitimes peuvent établir 
leur crédibilité et améliorer l'envoi des courriers électroniques.


## Comment ça fonctionne ?

Lorsqu'un courrier électronique voyage des serveurs de l'expéditeur à la boîte de réception du destinataire, il passe par différents serveurs intermédiaires. 
Ce parcours expose le courrier électronique à des vulnérabilités potentielles, telles que des modifications non autorisées. DKIM agit comme une norme mondiale d'authentification des courriers électroniques, 
sécurisant ainsi le contenu du courrier de l'expéditeur au destinataire à l'aide d'une signature cryptographique du corps du message. Cette signature garantit que le domaine de l'expéditeur 
est légitime et que le message n'a pas été altéré pendant son transit. DKIM opère au niveau de la couche application du modèle OSI, offrant ainsi une protection pour les protocoles de messagerie tels que SMTP,
IMAP et POP, même lorsqu'ils sont utilisés de manière sécurisée.

Une fois qu'un courrier électronique est envoyé depuis un domaine configuré avec DKIM, le serveur de messagerie de l'expéditeur génère une signature DKIM en utilisant une clé privée associée à la clé publique 
enregistrée dans le DNS du domaine d'expédition. Cette signature est ajoutée au courrier électronique sous la forme d'un en-tête DKIM-Signature.


***
Voici un exemple de signature DKIM dans un mail.


<p align="center">
  <img width="460" height="300" src="https://github.com/Shayann37/Wiki-TI/assets/71373280/1f5b8c69-7d25-46ba-8965-07b0802e4e2b">
</p>

***

Lorsque le courrier électronique est reçu par le serveur de messagerie du destinataire, celui-ci extrait l'en-tête DKIM-Signature et utilise la clé publique récupérée à partir de l'enregistrement DKIM dans le DNS
pour vérifier la signature. Si la vérification réussit, cela confirme que le courrier électronique provient bien du domaine indiqué et qu'il n'a pas été modifié pendant le transit.
Cette vérification de l'authenticité du domaine de l'expéditeur offre une protection contre la falsification des adresses électroniques. Les destinataires peuvent avoir confiance que l'expéditeur 
est réellement le propriétaire légitime du domaine mentionné dans l'en-tête DKIM-Signature. Cela permet de réduire les risques liés au spam, à l'hameçonnage 
et à d'autres formes de courriers électroniques malveillants.

En plus de l'authentification de l'expéditeur, DKIM assure également l'intégrité du message. Si le contenu du courrier électronique est modifié en cours de route, 
la signature DKIM ne correspondra plus et la vérification échouera. Cela permet de détecter toute modification indésirable du message et d'alerter le destinataire.


## Comment configurer DKIM ?

* La première étape consiste à générer une paire de clés DKIM composée d'une clé privée et d'une clé publique. La clé privée est utilisée pour signer les courriers électroniques sortants, 
tandis que la clé publique est placée dans le DNS pour permettre la vérification des signatures par les serveurs de messagerie entrants. Vous pouvez utiliser des outils tels que [Dkim Core Tool](https://dkimcore.org/tools/)
ou d'autres outils spécifiques à votre serveur de messagerie pour générer les clés.

Exemple d'une clé privée :

![cléPrivée](https://github.com/Shayann37/Wiki-TI/assets/71373280/3ba86e12-28ef-4fc1-8b0d-fb6a3a36f127)

Exemple de clé publique : 

![cléPublique](https://github.com/Shayann37/Wiki-TI/assets/71373280/ae201da0-149a-4a6c-9666-0fed72254255)

***

* Une fois les clés générées, vous devez configurer votre serveur de messagerie pour utiliser DKIM. Les étapes précises varient en fonction du serveur de messagerie que vous utilisez. En général, 
vous devrez ajouter des lignes de configuration spécifiques à DKIM dans les fichiers de configuration de votre serveur de messagerie.

Voici un exemple de commande et de configuration que vous devez faire pour un serveur mail utilisant "Postfix" : 

1. Installer le paquet "OpenDkim" sur votre système
```code 
apt-get install opendkim opendkim-tools
```

2. Ajouter les lignes suivantes dans le fichier de configuration Postfix
```code 
milter_default_action = accept
milter_protocol = 2
smtpd_milters = inet:localhost:8891
non_smtpd_milters = inet:localhost:8891
```

3. Ajouter les lignes suivante dans le fichier "/etc/opendkim.conf"
```code 
Domain example.com                //example.com doit évidement être changé par votre propre nom de domaine
KeyFile /path/to/private/key      //changer le chemin par votre chemin d'accès à votre clé privée
Selector default
```

4. Définir la variable SOCKET dans le fichier "/etc/default/opendkim"
```code
SOCKET="inet:8891"
```

Si vous désirez avoir une explication et une démonstration plus approfondie sur la configuration de DKIM, n'hésitez pas à jeter un coup d'oeil sur ce [lien](https://fr-wiki.ikoula.com/fr/Installer_DKIM_sur_Postfix_sous_Debian).

***


* Maintenant votre serveur de messagerie configuré, vous devez ajouter un enregistrement DKIM au système DNS associé au domaine d'expédition. 
Cet enregistrement est généralement un enregistrement TXT qui contient les informations de la clé publique DKIM, y compris le sélecteur DKIM. 
Vous devez copier le contenu de la clé publique générée précédemment dans cet enregistrement TXT.

***

* Dès la configuration effectuée, il est recommandé de tester et de vérifier que DKIM fonctionne correctement. Vous pouvez envoyer des courriers électroniques depuis votre domaine 
configuré avec DKIM vers des adresses externes et vérifier les en-têtes des courriers électroniques reçus pour voir si les signatures DKIM sont présentes et valides (image d'exemple observable dans 
la partie "Comment ça fonctionne ?). 
Vous pouvez également utiliser des outils en ligne tel que [MxToolBox](https://mxtoolbox.com) pour vérifier la configuration DKIM de votre domaine.

## Conclusion

En résumé, DKIM est une méthode d'authentification qui ajoute des signatures cryptographiques aux messages électroniques, protégeant ainsi contre la falsification des adresses électroniques.
En permettant au serveur de messagerie du destinataire de vérifier l'authenticité du domaine de l'expéditeur, DKIM renforce la sécurité des courriers électroniques,prévient la manipulation 
des messages et réduit les risques de spam et d'attaques de phishing.

# Sources

* Mantra, [SPF, DKIM et DMARC : pourquoi sont-ils importants et comment les configurer ?](https://fr.mantra.ms/blog/spf-dkim-dmarc-explication-parametrage). Consulté le 26 mai 2023

* Wikipédia, [DomainKeys Identified Mail](https://fr.wikipedia.org/wiki/DomainKeys_Identified_Mail). Consulté le 26 mai 2023

* Youtube, Docteur Internet, [Le DKIM c'est quoi ?](https://www.youtube.com/watch?v=nlAx8WMM5Z4). Consulté le 26 mai 2023

* DigitaWeb, Corentin Jacquemin, [DKIM : Comment le configurer ?](https://www.digitaweb.com/blog/dkim-comment-configurer). Consulté le 26 mai 2023

* Hostinger Tutorials, Roua K., 18 avril 2023, [C’est quoi DKIM ? Le Guide du Débutant (2023)](https://www.hostinger.fr/tutoriels/enregistrement-dkim). Consulté le 26 mai 2023

* Ikoula, [Installer DKIM sur Postfix sous Debian](https://fr-wiki.ikoula.com/fr/Installer_DKIM_sur_Postfix_sous_Debian). Consulté le 26 mai 2023

* OpenAi, [ChatGTP](https://chat.openai.com). Consulté le 26 mai 2023

***

Fait le 26 mai 2023 par GONZALEZ Shayann 2TL2

