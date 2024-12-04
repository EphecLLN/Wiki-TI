---
layout: default
title: Open Relay
parent: Réseaux
---

# Open Relay 

## Introduction [^1]

Un Open Relay ou relais de courrier ouvert est un serveur SMTP configuré pour permettre à tout le monde sur internet d'envoyer un mail à n'importe quel utilisateur par le billet de ses serveurs.
Au début d'internet, c'était la configuration par défaut de nombreux serveurs de courrier, mais cette configuration à vite changer face à sa fragilité contre les hackers, les spammeurs et également certain vers. Par la suite, de nombreux relais sont passés sur des configurations fermées ou on utiliser des blacklists contre les serveurs ouverts.

## Serveur de messagerie [^2]

Un serveur de messagerie reçoit, traite et transmet des courriers électroniques, il gère la "livraison" des mail en ayant un rôle important dans la sécurité.
Les serveurs de messages utilisent le protocole SMTP pour l'envoi de mail et POP3 ou IMAP pour les recevoir. SMTP achemine les messages vers leurs destinations via des serveurs sortants et IMAP et POP3 offre la possibilité à l'utilisateur d'accéder sur ses appareils à ses mails sur un serveur de messageries.
La sécurité est un enjeu important pour les serveurs de messageries. Le cryptage des messages durant l'échange permet d'assurer la confidentialité des données personnelles et professionnelles qui se trouveraient dans les mails.
![image](https://github.com/quentinrld/Wiki-TI/assets/71373028/d0e11e71-5be5-4a5e-b6a9-bbd860a69b90)
image tirer de [^6]

## Open Relay [^2] [^3]

Un Open Relay est un serveur de messagerie configuré pour permettre l'envoi d'e-mails sans vérifications de l'identité de l'expéditeur et du destinataire. Cela signifie que ces serveurs permettent le transfert de mails sans contrôler de manière précise la source du message.
En opposition au serveur sécurisé, qui met en place une vérification de l'authenticité des différents tiers concerné par un e-mail avant de le traiter, un Open Relay quant à lui ne met en place aucune de ces mesures de sécurité. Tout ceci ouvre le champ des possibles aux utilisations abusives du serveur.
Un Open Relay est donc un point d'accès facile pour les spammeurs ou autres personnes malveillantes qui exploite ceci pour envoyer des courriers indésirables à grande échelle. Ces courriers peuvent être de nature différente telle que des spam commerciaux, des messages frauduleux et ça sans aucune contrainte. 
![image](https://github.com/quentinrld/Wiki-TI/assets/71373028/b8d76f5b-c20c-497e-a7e8-78718cbfb077)
image tirer de [^6]

## Les risque liés aux Open Relays [^4] [^7]

- Envoi massif de spam : les Open Relays offrent aux spammeurs la possibilité d'envoyer en grandes quantités des messages sans restriction. Cela sature les boites mail, fait perdre du temps et nuis au bon fonctionnement des filtres antispam.
- Attaque de phishing plus simple : les Open Relays étant accessibles facilement par les personnes malveillantes, ils ont la possibilité d'envoyer des e-mails à l'apparence officielle, mais qui cache un mécanisme pour récupère vos données.
- Vulnérabilité aux attaques DDoS : toujours grâce aux failles et à la non-vérification des messages envoyé, les personnes malveillantes peuvent envoyer de grandes quantités de courrier à une destination précise inondant alors le serveur et le mettant temporairement inaccessible.
- Propoagation de logiciels malveillants : pareil pour le contenu des e-mails qui n'est pas contrôlé et donc qui ouvre les portes aux malware. Les e-mails peuvent comprendre des liens ou pièces jointes vers des logiciels malveillants.
- Mauvaise réputation : un serveur étant configuré en open relay peuvent être victime d'une mauvaise réputation et risque d'être assez rapidement référencer dans des listes de blocage. Ces listes identifient les serveurs étant associés au spam ou autres activités du genre.
- Impact sur les performances : étant donné le grand nombre de messages suite au spam, les performances de ces serveurs se voient réduites pour les e-mails légitimes.
- Non-conformité aux politiques de sécurité : l'utilisation d'open relay va à l'encontre des bonnes pratiques de sécurité et des politiques des fournisseurs de services. En outre, en maintenant un open relay, les normes de sécurité et de protections des données ne sont pas respectés.

## Préventions contre les Open Relays [^5] [^4]

Les différents fournisseurs de messagerie mettent en place différentes mesures afin de prévenir des open relays et assurer le bon fonctionnement de leur système.

- Authentification des expéditeurs : les fournisseurs de services de messagerie exigent souvent une authentification appropriée des expéditeurs avant de leur permettre de relayer des emails. Cela peut se faire par le biais de protocoles d'authentification tels que SPF, DKIM et DMARC.
- Utilisation de listes de contrôle d'accès : les ACL sont utilisées pour définir des règles qui contrôlent les adresses IP autorisées à relayer des emails via le serveur. Les fournisseurs de services de messagerie configurent des ACL pour restreindre le relais aux seules adresses IP approuvées, empêchant par la même l'utilisation abusive d'Open Relays.
- Filtrage des emails suspects : les systèmes de messagerie intègrent des mécanismes de filtrage pour détecter et bloquer les emails suspects ou non conformes. Des techniques telles que la détection de spam, l'analyse des en-têtes et le filtrage basé sur des listes noires sont utilisées pour identifier les emails indésirables.
- Politiques strictes pour le relais d'emails : les fournisseurs de services de messagerie appliquent des politiques strictes pour le relais d'emails. Cela peut inclure des règles sur les volumes d'emails envoyés par utilisateur, les restrictions de destinataires, et d'autres paramètres de contrôle visant à éviter les abus.
- Surveillance et analyse du trafic de messagerie : des outils de surveillance et d'analyse du trafic sont utilisés pour identifier les schémas de comportement indésirables, tels qu'un volume élevé d'emails sortants provenant d'une seule adresse IP.
- Mise à jour régulière des logiciels : les fournisseurs de services de messagerie maintiennent leurs serveurs de messagerie à jour en appliquant régulièrement les correctifs de sécurité et les mises à jour des logiciels.

## Bonne pratiques pour les serveurs de messageries [^4]

- Mettre à jour régulièrement les logiciels : Assurez-vous de maintenir votre serveur de messagerie à jour en installant les correctifs de sécurité et les mises à jour logicielles.
- Configurer correctement les paramètres de relais : Veillez à configurer les paramètres de votre serveur de messagerie de manière à n'accepter les emails en relais que pour les domaines ou les utilisateurs autorisés. Restreignez le relais uniquement aux adresses IP autorisé et définissez des politiques strictes.
- Utiliser l'authentification des expéditeurs : Mettez en place des mécanismes d'authentification tels que SPF, DKIM et DMARC pour vérifier l'authenticité des expéditeurs. Cela permet de bloquer les emails provenant d'adresses usurpées et de renforcer la sécurité du relais.
- Mettre en place un filtrage efficace : Utilisez des filtres de messagerie pour détecter et bloquer les emails indésirables ou suspects. Cela inclut le filtrage du spam, la vérification des signatures DKIM et l'utilisation de blacklist.
- Surveiller le trafic de messagerie : Mettez en place des outils de surveillance et d'analyse du trafic de messagerie pour détecter les activités suspectes ou anormales. Surveillez les volumes d'emails sortants, les adresses IP suspectes et les schémas de comportement indésirables pour identifier rapidement tout problème potentiel.
- Limiter les droits d'accès : Accordez uniquement les droits d'accès nécessaires aux utilisateurs et aux processus du serveur de messagerie. Restreignez les autorisations de modification des paramètres de relais et des configurations pour empêcher toute modification non autorisée.
- Mettre en place des journaux d'activité : Activez les journaux d'activité du serveur de messagerie pour enregistrer les événements liés aux emails, y compris les tentatives de relais, les erreurs de livraison et les activités suspectes.
- Effectuer des audits de sécurité réguliers : Réalisez des audits de sécurité réguliers de votre infrastructure de messagerie pour identifier les éventuelles vulnérabilités ou erreurs de configuration. Cela permet de prendre des mesures correctives appropriées pour renforcer la sécurité.

## Impacte des Open Relays [^7]

Ils ont un grand impact sur la société dans son ensemble. Ils violent votre vie privée en facilitant l'envoi de spams et d'e-mails frauduleux, compromettant ainsi votre sécurité et votre confidentialité. De plus, ces relais ouverts encombrent les réseaux de communication, génèrent de gros volumes d'e-mails et retardent la livraison de messages légitimes.   
Ces pratiques affectent également la confiance des utilisateurs dans leur système de messagerie. Confrontés à un barrage constant de spams et de courriers indésirables, les utilisateurs peuvent devenir réticents à faire confiance aux e-mails légitimes, ce qui entraîne une communication inefficace et des relations commerciales perturbées. De plus, les relais ouverts augmentent les risques de sécurité en fournissant une plate-forme permettant aux cybercriminels de mener des attaques de phishing, de distribuer des logiciels malveillants et de compromettre les systèmes informatiques. Cela expose les utilisateurs et les organisations à un risque accru de vol de données, de perte financière et d'atteinte à la réputation.   
L'impact va au-delà des utilisateurs individuels. Les entreprises qui sont victimes de relais ouverts encourent des coûts importants associés à la lutte contre le spam, à la protection contre les attaques de phishing et à la maintenance de leur infrastructure de messagerie. Ces coûts comprennent la mise en place de systèmes de sécurité avancés, le nettoyage des boîtes de réception et la perturbation de la productivité des employés. De plus, les fournisseurs de services de messagerie sont confrontés au défi de lutter contre le spam. L'identification et le blocage efficaces des e-mails indésirables nécessitent une amélioration continue de la technologie de filtrage, ce qui nécessite un investissement important en termes de temps, de ressources et de développement technologique.   
Après tout, les entreprises qui utilisent leurs serveurs de messagerie comme relais ouverts peuvent subir d'énormes dommages à leur réputation. Les utilisateurs qui combinent ces pratiques avec une mauvaise sécurité des données et des contrôles de confidentialité peuvent perdre confiance en ces entreprises.    
Par conséquent, il est essentiel que les utilisateurs, les entreprises et les fournisseurs de services de messagerie travaillent ensemble pour lutter contre le relais ouvert et réduire son impact négatif sur la société dans son ensemble. Cela comprend la mise en œuvre de mesures de sécurité efficaces, la sensibilisation des utilisateurs et la poursuite des investissements dans la technologie et la formation pour prévenir ces pratiques nuisibles et protéger l'intégrité des communications électroniques.    

## Conclusion
 
Cet article met en évidence les risques et les ramifications des open relays dans le contexte de l'architecture de messagerie. Cela souligne l'importance de prendre des mesures de sécurité appropriées telles que : Cela inclut l'authentification des expéditeurs, le filtrage des e-mails suspects et la configuration des paramètres de relais en conséquence. La sécurisation des serveurs de messagerie est essentielle pour empêcher les open relays, protéger la confidentialité, maintenir la confiance des utilisateurs et réduire le risque de spam, de phishing et d'attaques malveillantes.

## Bibliographie 

[^1] "_Open relay_" wikipedia.org https://fr.wikipedia.org/wiki/Open_relay (consuté le 25/05/2023)
  ** Résumé : Page décrivant l'open relay tres simplement 
  ** Avis : Très peu détailler mais correct pour une intro   
[^2] "_Serveur de messagerie_" one.com https://www.one.com/fr/email/quest-ce-quun-serveur-de-messagerie (consuté le 25/05/2023)
  ** Résumé : Description des serveurs de messagerie et quelque point de sécurité
  ** Avis : Très bon site avec un conenu bien développer 
[^3] "_Qu'est ce qu'un relais ouvert_" ionos.fr https://www.ionos.fr/assistance/serveurs-et-cloud/serveur-dedie-pour-les-serveurs-achetes-avant-le-28102018/serveur/quest-ce-quun-relais-ouvert-open-mail-relay/ (consuté le 25/05/2023)
  ** Résumé : Articles reprenant différent points des open relays
  ** Avis : Article cours mais intéressant 
[^4] "_Sécurité messagerie_" clusif.fr https://clusif.fr/wp-content/uploads/2015/09/securite_messagerie.pdf (consulté le 26/05/2023)
  ** Résumé : Présision sur les attaques et protections des messageries 
  ** Avis : Attention que c'est un article de 2005 mais sinon le contenu est intéressant et bien expliqué 
[^5] "_Authentification de courrier_" powerdmarc.com https://powerdmarc.com/fr/qu'est-ce-que-l'authentification-du-courrier-%C3%A9lectronique/
  ** Résumé : Comment authentifier un mail 
  ** Avis : très bon articles 
[^6] "_What's open relay_" techslang.com https://www.techslang.com/definition/what-is-open-relay/ (consulté le 26/05/2023)
  ** Résumé : explication global des open relay 
  ** Avis : très intéressant et bien expliquer 
[^7] "_open mail relay_" duocircle.com duocircle.com/content/mail-relay-smtp/open-mail-relay (consulté le 26/05/2023)
  ** Résumé : Explication de la liberté des spammeur avec les open relay  
  ** Avis : intéressant et bonne information
[] ChatGPT https://chat.openai.com/ (consulté le 26/05/2023)
  ** Avis : A utilisé avec précaution pour obtenir les grandes idées sur le sujet.
