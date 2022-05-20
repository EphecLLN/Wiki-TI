# Etude de cas : Compromission d'un VPS 

En mai 2022, un des VPS utilisé dans le cadre de ce cours a été compromis : nous avons reçu un email de notification de la part du fournisseur indiquant que des tentatives d'intrusion sur le port SSH ont été commises depuis cette machine.  Suite à cela, nous avons investigué la machine incriminée pour tenter d'en savoir concernant l'attaque subie, en nous inspirant de pistes proposées sur le [site Linux Hint](https://linuxhint.com/determine_if_linux_is_compromised/) [1].  

## Observation des connexions réseaux

Pour visualiser les tentatives de connection lançées depuis la machine, le bon vieil outil netstat est toujours utile.  Contrairement au débugging de services, nous n'utilisons dans un premier temps pas l'option "l", car ce ne sont pas les ports "en écoute" qui nous intéressent, mais bien les connexions initiées depuis la machine.  On peut voir dans l'output ci-dessous qu'il y a énormément de tentatives de connexion sur des ports 22 de machines distantes (ce qui correspond bien à la plainte reçue), et que ces tentatives de connexion ont parfois réussi (état ESTABLISHED).  Pour les tentatives réussies, on voit que le coupable est le processus de pid 150510, associé à l'exécutable tsm.  


    $ sudo netstat -antp | more
    Active Internet connections (servers and established)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
    tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      54381/systemd-resol 
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      53106/sshd: /usr/sb 
    tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      133699/nginx: maste 
    tcp        0      0 152.228.217.108:41524   94.26.246.51:22         TIME_WAIT   -                   
    tcp        0      0 152.228.217.108:41688   45.192.85.204:30945     TIME_WAIT   -                   
    tcp        0      0 152.228.217.108:55360   46.4.90.59:22           ESTABLISHED 150510/tsm          
    tcp        0      0 152.228.217.108:33044   91.121.71.10:22         TIME_WAIT   -                   
    tcp        0      0 152.228.217.108:52626   46.254.16.41:22         TIME_WAIT   -                   
    tcp        0      0 152.228.217.108:59622   212.122.43.13:22        TIME_WAIT   -                   
    tcp        0      0 152.228.217.108:55436   141.79.10.62:22         TIME_WAIT   -                   
    (etc.)


Nous pourrions également regarder les ports actifs depuis la machine, cette fois avec l'option -l.  Cela permettrait de vérifier s'il n'y a pas un service "suspect" à l'écoute, par exemple en attente de commandes depuis la machine contrôlant l'attaque.  Ce n'est pas le cas dans la situation analysée. 

Pour vérifier les dernières tentatives de login, on peut consulter le fichier .bash-history, soit directement, soit en utilisant la commande "last". Cependant, les attaquants nettoient souvent ce fichier après l'intrusion.  Dans notre cas, aucune information utile n'y figure.  

## Analyse du processus incriminé

Une fois le processus coupable identifié (dans notre cas, tsm), on peut en savoir plus à son sujet avec un ps -A ou bien un "top", comme le montre l'output ci-dessous. On voit que le processus tsm tourne avec l'utilisateur "node", et qu'il occupe une belle portion du CPU du VPS.  Il s'agit donc de l'utilisateur qui a été compromis.  On observe également un autre processus lancé par node et utilisant beaucoup de ressources : kswapd0 

    top - 08:53:08 up 3 days, 22:39,  1 user,  load average: 43.06, 24.71, 21.26
    Tasks: 114 total,   2 running, 112 sleeping,   0 stopped,   0 zombie
    %Cpu(s): 92.4 us,  3.7 sy,  0.0 ni,  0.0 id,  0.0 wa,  0.0 hi,  4.0 si,  0.0 st
    MiB Mem :   1935.3 total,    145.7 free,    618.3 used,   1171.3 buff/cache
    MiB Swap:      0.0 total,      0.0 free,      0.0 used.   1125.8 avail Mem 

        PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                                                               
     152194 node      20   0 4783328  12848   2372 S  81.4   0.6   3:05.02 tsm                                                                                                   
     103921 node      20   0  714124 267052   2360 S  16.6  13.5 766:23.31 kswapd0                                                                                               
      54387 root      19  -1   84148  21452  20232 S   0.3   1.1   0:33.83 systemd-journal                                                                                       
     153042 root      20   0   12172   6964   6144 S   0.3   0.4   0:00.01 sshd                                                                                                  
          1 root      20   0  170836  11388   6816 S   0.0   0.6   0:36.37 systemd                                                                                               
          2 root      20   0       0      0      0 S   0.0   0.0   0:00.01 kthreadd                                                                                              
          3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp                                                                                                
          4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_par_gp                 


Quelques recherches Google concernant ces processus nous ont mené à [cet article](https://qastack.fr/ubuntu/1115770/crond64-tsm-virus-in-ubuntu) [2].  Suite à la lecture, nous avons pu vérifier que le compte utilisateur node avait effectivement une clé SSH autorisée dans son .ssh/authorised_keys.  Cela permet à l'attaquant de se reconnecter à volonté sans mot de passe. D'autres recherches mènent à l'identification du processus kswapd0 comme dissimulant un malware de mining. 


A ce stade, les investigations ont été arrêtées et le problème résolu par une réinitialisation complète du VPS, avec demande à l'étudiant de mettre en place directement l'authentification par clé, la désactivation du ssh par pwd, ainsi que Fail2Ban afin d'éviter de nouvelles compromissions.  D'autres éléments auraient pu être investiguées sur base des pistes proposées par U. Asad [1], telles que les crontabs ou les logs, entre autres.  

Il aurait été possible de tenter de nettoyer le VPS en supprimant l'utilisateur node ainsi que les traces du malware, mais dans ce cas précis, puisqu'il n'était pas nécessaire de sauver des données, une réinitialiasion est plus simple et plus sûre car plus radicale. 

## Bibliographie
[1] Usama Asad, [How to determined if a linux system is compromised](https://linuxhint.com/determine_if_linux_is_compromised/), 2020, consulté le 20 mai 2022
  - Résumé : Article type "blog" en anglais listant des commandes/procédures permettant d'investiguer une éventuelle compromission sur un serveur Linux. 
  - Avis sur la ressource : Excellent article détaillant et expliquant de manière claire et assez complète des pistes génériques à suivre en cas de compromission.  

[2] Auteur anonyme [virus crond64 / tsm dans Ubuntu](), forum "QAStack", date du post inconnue, consulté le 20 mai 2022. 
  - Résumé : Post de forum témoignant de l'analyse d'une infection d'une machine Ubuntu par un processus tsm. 
  - Avis sur la ressource : Partage d'expérience d'un utilisateur particulier décrivant son cas spécifique.  Donne des pistes intéressantes pour investiguer des situations similaires. 
