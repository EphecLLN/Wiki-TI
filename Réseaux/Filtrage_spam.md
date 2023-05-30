# Importance du filtrage de spam dans les services de messagerie
Malgré la diminution des courriers indésirables ces dernières années (de +- 90% à +- 50% des mails), la quantitée de courrier indésirable reste un problème. C'est pourquoi il est essentiel de setup des mécanismes de filtrage qui vont être efficaces pour avoir une expérience utilisateur positive mais aussi pour protéger les utilisateurs contres les menaces du spam.

![Diminution_spam](https://www.altospam.com/actualite/wp-content/img/2019/05/stats-spam-05-2019-1024x667.png)

Le spam se réfère aux messages généralement commerciaux qui sont envoyés par grand nombre à beaucoup d'utilisateurs en même temps. Ils peuvent inclure de la pub, des offres frauduleuses, escroqueries, contenir des logiciels malveillants ou encore des liens vers de sites web dangereux.

C'est la raison pour laquelle le filtrage de spam est un "outil" essentiel des services de messagerie. Il cherche en fait à indentifier et à séparer les e-mails habituels des courriers indésirables, ce qui permet de cette manière aux utilisateurs de se focus sur les messages importants et éviter le spam de leur faire perdre du temps où encore d'être atteints par des mails contenant des menaces.

# Les composants du système de messagerie et leur rôle dans le filtrage de spam

## MUA (Mail User Agent)
Le MUA est le composant du système de messagerie qui permet aux utilisateurs d'accéder et rédiger un mail pour l'envoyer au MTA/MSA mais aussi à organiser les mails et intègre souvent des filtres de spam.

### Interaction avec les filtres de spam
L'un de ses rôles est de travailler avec les filtres de spam pour aider à identifier et à gérer les e-mails indésirables. 
Les MUA ont souvent des outils d'aide de filtrage de spam qui utilisent plusieurs techniques pour calculer la probabilité qu'un e-mail soit du spam. Ce n'est pas le MUA qui filtre les spams mais il utilise des techniques pour aider à filtrer les messages. 
* L'analyse des en-têtes et contenus:
Les MUA scannent si demandé les en-têtes et les contenus des e-mails pour trouver des signes de spam. 
Ca inclut des en-têtes de courrier électronique qui sont suspects, des liens ou images suspects, et même des mots qui sont souvent utilisé dans les spams.
* L'analyse des Black lists:
C'est l'utilisation des listes noires qui répertorient les IP ou les domaines connus pour envoyer du spam.
Les MUA peuvent consulter ces listes pour vérifier si l'expéditeur d'un e-mail est répertorié justement dans ces listes, ce qui peut contribuer à l'identification des spams.
* Règles de filtrage personnalisées:
Elles permettent aux utilisateurs de marquer et gérer les e-mails. Par exemple, l'utilisateur peut s'il le souhaite créer une règle/filtre pour marquer les mails provenant d'un adresse spécifique comme spam.

L'utilisateur peut jouer un rôle important dans le filtre de spams parce-que il fournit des infos importantes aux filtres pour les aider à améliorer leurs critères de détection. Ils peuvent signaler les spams avec le MUA.

## MSA (Mail Submission Agent)
Le MSA est le composant du système de messagerie responsable de la soumission des e-mails. Il intervient lorsqu'un utilisateur envoie un e-mail à partir de son MUA pour qu'il soit transmis aux destinataires.

Même si le MSA est principalement chargé de la transmission des e-mails, il peut aussi jouer un rôle dans le filtrage de spam.

### Techniques de filtrage de spam
Le MSA peut s'aider de techniques de filtrage de spam pour mesurer le danger des e-mails soumis avant de les transmettre aux destinataires (d'abord au MTA). 
Ces techniques peuvent inclure l'analyse des en-têtes, du contenu et d'autres caractéristiques des e-mails pour détecter les signes de spam. Le MSA peut utiliser des mécanismes tels que les listes noires, les règles de filtrage, l'analyse heuristique et d'autres méthodes pour évaluer la chance qu'un e-mail soit du spam.

### Validation des règles de filtre avant l'envoi des e-mails
Avant de transmettre les e-mails aux destinataires, le MSA peut valider les règles de filtrage de spam configurées. 
Ces règles peuvent être données par l'administrateur du système de messagerie. Ils spécifient les critères à utiliser pour identifier les e-mails indésirables. Le MSA vérifie si les e-mails soumis sont OK par rapport aux critères donnés dans les règles de filtrage. Si un e-mail est identifié comme spam en fonction de ces règles, le MSA peut prendre le rejeter de l'e-mail et le placer dans le dossier de spam.

## MTA (Mail Transfer Agent)
### Rôle dans le filtrage de spam
Le MTA joue un rôle dans le filtrage de spam lors de la transmission des e-mails. En fait, il est considéré comme l'élément le plus important du système de messagerie en ce qui concerne le filtrage de spam.

Le principal rôle du MTA est de transférer les e-mails d'un serveur à un autre. Lorsqu'un e-mail est envoyé depuis le MUA (Mail User Agent) d'un expéditeur ou venant d'un MSA, il est responsable de son acheminement vers le MTA du destinataire. Cependant, avant de transmettre l'e-mail au destinataire, le MTA applique des techniques de filtrage de spam pour mesurer la qualité de l'e-mail.
### Utilisation de techniques de filtrage
Quand le MTA effectue le filtrage de spam lors de la transmission des e-mails, il peut utiliser plusieurs techniques pour évaluer la probabilité qu'un e-mail soit du spam.

Quelques techniques et leur mécanisme :

1. Filtres basés sur les politiques de sécurité :
Les politiques de sécurité sont les règles et les critères de filtrage à appliquer en fonction des préférences utilisateur. 
Ces politiques peuvent être des règles comme l'interdiction d'e-mails venant de certaines adresses IP ou de domaines ou aussi le blocage de certains types de pièces jointes dangereuses ou encore l'exigence d'une authentification SPF ou DKIM pour vérifier l'origine des e-mails.

2. Analyse heuristique :
C'est une technique de filtrage qui utilise des algorithmes d'apprentissage automatique pour détecter les spams. Comme vu précédemment, Le MTA peut analyser les caractéristiques des e-mails (mots-clés suspects, structures de phrases inhabituelles, liens ou d'images suspectes,...) et les comparer à des modèles de mails spam pour savoir si le mail est du spam ou non. 

3. Listes de réputation :
Les listes de réputation fournissent des infos sur la réputation du coup des expéditeurs en se basant sur des critères: l'historique de l'adresse IP, la conformité aux normes de messagerie, la fréquence de l'envoi de spam, etc... 
Le MTA peut consulter ces listes pour savoir à quel point ou pas faire confiance à l'expéditeur. 
Si l'expéditeur est enregistré comme ayant une mauvaise réputation en raison de l'envoi de spam, le MTA peut augmenter la chance que l'e-mail soit classé comme spam.

4. Analyse de contenu et de structure :
Comme le MUA, Le MTA peut analyser le contenu et la structure des e-mails pour trouver des caractéristiques communes aux spams. L'analyse inclut en-têtes, mots-clés spécifiques, structures de phrases inhabituelles, des liens ou images suspects.

5. Blacklist:
Lorsqu'un MTA reçoit un e-mail du MSA ou MUA, il peut vérifier ces listes noires pour voir si l'expéditeur ou l'adresse IP source de l'e-mail est répertorié. 
Si oui ca indique que l'expéditeur est considéré comme non fiable ou a été signalé pour l'envoi de spam.
La présence d'une adresse IP ou d'un domaine dans une liste noire peut elle aussi déclencher plusieurs actions de filtrage, telles que le rejet pur et simple de l'e-mail, le marquage comme spam, ou une évaluation supplémentaire à l'aide d'autres techniques de filtrage.
Les listes noires peuvent venir de différentes sources comme par exemple les organisations spécialisées dans la lutte contre le spam, des fournisseurs d'accès Internet ou des initiatives communautaires. Il existe des listes noires publiques et privées. Les administrateurs de MTA peuvent choisir celles qu'ils souhaitent utiliser en fonction de ce qu'ils ont besoin et de leurs politiques de sécurité.

6. Le filtrage bayésien:
Cette technique consiste à analyser le contenu des e-mails et à calculer la probabilité qu'ils soient des spams. Comment il fait, c'est assez simple: le MTA utilise un modèle de classification bayésienne déja entraîné.
Le modèle est construit en se basant sur pleins d'e-mails d'apprentissage préalablement catégorisés comme spams ou pas. 
À partir de ces exemples, le MTA définit "les probabilités conditionnelles des différentes caractéristiques présentes dans les e-mails".


En utilisant ces techniques, le MTA est en position de détecter et bloquer les mails indésirables avant qu'ils n'atteignent le ou les destinataires finales. Cela contribue à maintenir la qualité et la sécurité du service de messagerie en empêchant le spam.

## MDA (Mail Delivery Agent)
## Stockage des mails
Il joue aussi un rôle dans le filtrage de spam lors de la transmission des e-mails, ainsi que dans le stockage des e-mails. Une fois que les utilisateurs ont été définis, il est important de spécifier où et comment les e-mails sont stockés, de manière à ce que le MDA puisse y accéder pour les déposer.

En ce qui concerne le stockage des e-mails, deux formats sont souvent utilisés: le format mbox et le format Maildir. 

Le format mbox est un format de boîte mail Unix, dans laquelle tous les e-mails sont concaténés dans un seul fichier. 
Malheureusement ce format est vraiment pas pratique quand la boîte devient assez volumineuse parce-qu'il faut alors logiquement parcourir tout les fichiers pour accéder à un mail. 
En plus, lorsqu'un e-mail est déposé ou consulté, le fichier doit être bloqué en écriture pour éviter les problèmes de cohérence.

En revanche, le format Maildir a été proposé à l'origine pour le serveur QMail. Dans ce format, chaque e-mail est stocké dans un fichier différent et chaque message est placé dans un répertoire dédié. Contrairement au format mbox il est plus éfficace car il permet un accès individuel à chaque message sans avoir besoin de parcourir un fichier entier. 
Cela facilite le traitement et la manipulation des e-mails, tout en offrant de meilleures performances en termes d'accès et de gestion des boîtes aux lettres.

### Rôle dans le filtrage de spam
En ce qui concerne le filtrage de spam, le MDA peut aussi encore une fois appliquer des règles et des mécanismes de filtrage (analyse des en-têtes, du contenu et des caractéristiques des e-mails pour détecter les signes de spam)
Le MDA peut aussi interagir avec d'autres composants du système de messagerie dont MTA pour appliquer des stratégies de filtrage à différentes étapes du processus d'envoi de mails.

## MRA (Mail Retrieval Agent)
### Interaction avec les filtres de spam lors de la récupération des e-mails par les utilisateurs
 Lorsque le MRA récupère les e-mails du serveur de messagerie, il peut interagir avec le filtres de spam de la classification des e-mails en tant que spam ou non spam pour appliquer des règles de filtrage supplémentaires.


# SpamAssassin pour Postfix
SpamAssassin est un en gros un outil de filtrage spam qui est open-source. 
Il utilise plusieurs techniques pour analyser les e-mails et déterminer s'ils sont du spam ou non. Dont des règles préconfigurées, des algorithmes d'apprentissage automatique et d'autres méthodes.

## Intégration de SpamAssassin avec Postfix
L'intégration de SpamAssassin avec Postfix peut se faire en configurant Postfix pour consulter la base de données de réputation de SpamAssassin quand il reçoit une demande de livraison d'un e-mail. 
Si l'adresse IP de l'expéditeur est répertoriée comme étant associée à du spam dans la base de données de réputation, Postfix peut refuser la livraison de l'e-mail, empêchant de cette manière l'e-mail spam d'atteindre le serveur de messagerie.

L'intégration se fait principalement au niveau du MTA et MDA. 

En ce qui concerne le MUA, la configuration de SpamAssassin peut varier.
Certains MUA peuvent avoir une intégration directe avec SpamAssassin, ce qui permet aux utilisateurs d'appliquer des règles de filtrage en plus au niveau du client pour marquer ou trier les e-mails en fonction de leur probabilité d'être du spam. 


# Bibliographie
1. Oktey, [Statistiques sur les spams et le phishing, les virus et ransomwares et les publicités](https://www.globalsecuritymag.fr/Statistiques-sur-les-spams-et-le,20190603,87692.html), juin 2019, consulté le 26/05/2023
2. Jyothiikaa Moorthy, [23 Email Spam Statistics to Know in 2023](https://www.mailmodo.com/guides/email-spam-statistics/), 24 Mars 2023, consulté le 26/05/2023
3. Wikipedia, [Spam](https://fr.wikipedia.org/wiki/Spam), 4 avril 2023, consulté le 26/05/2023
4. Wikipedia, [Email Spam](https://en.wikipedia.org/wiki/Email_spam), 9 février 2023, consulté le 26/05/2023
5. Yevgen Tsvetukhin, [What is an MTA?](https://mailtrap.io/blog/mail-transfer-agent/), 28 octobre 2019, consulté le 26/05/2023
6. The warmup inbox team, [MTA and spam](https://blog.warmupinbox.com/mail-transfer-agent/), 15 octobre 2021, consulté le 26/05/2023
7. Emma, [Everything about spam filters](https://myemma.com/blog/everything-you-ever-wanted-to-know-about-spam-filters/), 2015, consulté le 26/05/2023
8. Ivan LaBianca, [How spam filters work](https://www.theseventhsense.com/blog/how-spam-filters-work-and-how-to-stop-emails-going-to-spam), 4 mars 2022, consulté le 26/05/2023
9. itDepartement, [How email filtering works](https://www.your-itdepartment.co.uk/knowledge-bank/how-email-filtering-works/), 5 aout 2019, consulté le 26/05/2023
10. Wikipedia, [Maildir](https://fr.wikipedia.org/wiki/Maildir), 30 mars 2022, consulté le 26/05/2023
11. Wikipedia, [Mbox](https://fr.wikipedia.org/wiki/Mbox), 12 février 2023, consulté le 26/05/2023
12. Wikipedia, [Analyse heuristique](https://fr.wikipedia.org/wiki/Analyse_heuristique), 8 février 2021, consulté le 26/05/2023
13. Ionos, [Email blacklist ou blocklist, tout sur la liste noire anti-spam](https://www.ionos.fr/digitalguide/email/securite-email/quest-ce-que-le-blacklisting/), 6 février 2023, consulté le 26/05/2023
14. Maxime CHARLÈS, [Filtrage bayésien : technique pour lutter contre le SPAM](https://blog.provectio.fr/filtrage-bayesien-technique-pour-lutter-contre-le-spam/), 1er janvier 2018, consulté le 26/05/2023
15. Wikipedia, [Filtrage bayésien du spam](https://fr.wikipedia.org/wiki/Filtrage_bay%C3%A9sien_du_spam), 7 mars 2023, consulté le 26/05/2023
16. Wikipedia, [SpamAssassin](https://fr.wikipedia.org/wiki/SpamAssassin), 16 mai 2023, consulté le 26/05/2023
