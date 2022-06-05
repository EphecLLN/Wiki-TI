# Docker swarm et réseaux overlay

## Docker swarm
Un docker swarm est un ensemble ("cluster") d'hôtes Docker ("Docker hosts"). Un docker host est une instance Docker Engine. Il est possible d'en avoir plusieurs sur une même machine hôte. On appelles "nodes" les docker hosts faisant partie d'un swarm. Ces nodes peuvent avoir deux rôles : 

- manager : gère l'adhésion des hôtes et la délégation des taches.
- worker : fait tourner un service swarm.

Par défaut les nodes manager font aussi tourner des services comme des nodes worker. On peut les configurer pour qu'elles gèrent uniquement des taches de manager.

Le principe d'un docker swarm est de déployer les services sur un seule node, le manager. On fournit une définition de service au node manager et celui-ci s'occupera de départager les unités de travail, les taches ("tasks"), entre les différents worker nodes. 

### Créer un docker swarm et le rejoindre
On initialise sur un docker host le swarm. Ce docker host deviendra un manager.
``` 
docker swarm init --advertise-addr=<IP-MANAGER> 
```
\<IP-MANAGER> est  ...

La commande renvoie un message indiquant la commande à taper pour qu'un autre docker host puisse rejoindre le swarm en tant que worker ou manager. Elle ressemblera à :
```
docker swarm join --token <TOKEN> <IP-MANAGER>:2377 
```
On peut obtenir ces commandes en utilisant sur un node manager :
``` 
docker swarm join-token [worker/manager] 
``` 


On peut également changer changer le mode d'un node avec les commandes 
``` 
docker node promote <NODE-NAME> 
docker node demote <NODE-NAME> 
```
 La première transforme un worker en manager et la seconde fait l'inverse.

### Services et tasks
Un services est la définition des tasks à exécuter sur les nodes worker ou manager. En définissant un service on indique quelle image utiliser et les commandes à exécuter dans les tasks. On définit pour ce service un état optimal que Docker essaiera de maintenir. On peut définir le nombre de répliques, ressources de stockage, ressources de réseau, ports exposés vers l'extérieur, etc. Le manager distribuera les tasks en fonction de l'état optimal.

Les tasks, en opposition aux "standalone containers", sont des containers faisant partie d'un service swarm et étant gérés par un swarm manager. Le type de service qu'on définit dans l'état optimal permets de déterminer le nombre de tasks qui seront distribuées par le manager entre les workers.

- Replicated service : le manager distribue un nombre précis de tasks à travers les workers. 
- Global service : le manager distribue une task à chaque node disponible dans le cluster.

### Tolérance aux pannes et Load balancing 
Les workers communiquent l'état de leurs tasks qui leur ont été assigné. De cette manière le manager peut maintenir l'état désiré. Par exemple, dans le cas où un worker tombe en panne et n'est plus disponible le manager redistribue une nouvelle Task à un autre worker pour respecter le nombre de répliques qu'on doit avoir.

Le mode swarm possède un DNS interne qui assigne automatiquement une entrée DNS pour chaque service dans le swarm. Le manager fait du load balancing pour distribuer les requêtes entre les service disponibles en fonction du nom DNS du service. Il utilise le load balancing également pour rendre les services accessibles à l'extérieur du swarm si on le souhaite. Il peut assigner automatiquement un "Published Port", un port extérieur, à un service (entre 30 000 et 32 767). On peut aussi choisir nous même un port non utilisé.

## Réseaux overlay
