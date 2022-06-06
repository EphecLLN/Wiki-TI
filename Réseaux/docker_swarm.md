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
\<IP-MANAGER> est l'adresse IP de la machine abritant le manager.

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

### Création de services
La parmétrisation d'un service peut être très complète. Voyons les points basiques. Pour plus d'informations, n'hésitez pas à consulter la documentation [Docker](https://docs.docker.com/engine/swarm/services/#create-a-service).

La commande de base pour créer un service swarm est la suivante. \<IMAGE> est l'image qu'utiliserons les containers (tasks) et \<COMMAND> est la commande qui sera exécutée dans le container après sa création.
```
docker service create <IMAGE> <COMMAND>
```

On peut bien sûr compléter cette commande avec une multitude de flags :

-   ```--name ``` permets de donner un nom au service.
-   ```--p/--publish ``` permets de choisir un Published Port, un port externe sur lequel on accède au service.
-   ```--env ``` permets de définir des variables d'environnement.
-   ```--workdir``` permets de définir un working directory.
-   ```--user``` permets de définir un utilisateur.
-   ```--mode``` permets d'indiquer le mode de service (replicated, global, ...). Si on n'utilise pas ce flag un service sera replicated par défaut.
-   ```--replicas``` permets d'indiquer le nombre de répliques qu'on veut avoir.
- etc.

On peut également définir le comportement de mise à jour du service (délai avant mise à jour automatique, que faire lors d'une erreur de mise à jour, ...), définir une façon de revenir à une version antérieure du service en cas de problème avec la version actuelle du service.

Pour avoir des données persistantes on peut utiliser le flag ``` --mount ``` pour définir des data volumes ou bind mounts [(documentation docker)](https://docs.docker.com/engine/swarm/services/#give-a-service-access-to-volumes-or-bind-mounts). Le principe est similaire aux [bind mounts](https://docs.docker.com/storage/bind-mounts/) et [named volumes](https://docs.docker.com/storage/volumes/) utilisés pour avoir des données persistantes sur des standalone containers.

Quelques exemples issus de la documentation Docker :

```
docker service create --name helloworld \
					  --env MYVAR=myvalue \
					  --workdir /tmp \
					  --user my_user \
					  alpine ping docker.com
```
```
 docker service create --name my_web \
                        --replicas 3 \
                        --publish published=8080,target=80 \
                        nginx
```
```
docker service create \
					  --mode global \
					  --publish mode=host,target=80,published=8080 \
					  --name=nginx \
					  nginx:latest
```

### Tolérance aux pannes et Load balancing 
Les workers communiquent l'état de leurs tasks qui leur ont été assigné. De cette manière le manager peut maintenir l'état désiré. Par exemple, dans le cas où un worker tombe en panne et n'est plus disponible le manager redistribue une nouvelle Task à un autre worker pour respecter le nombre de répliques qu'on doit avoir.

Le mode swarm possède un DNS interne qui assigne automatiquement une entrée DNS pour chaque service dans le swarm. Le manager fait du load balancing pour distribuer les requêtes entre les service disponibles en fonction du nom DNS du service. Il utilise le load balancing également pour rendre les services accessibles à l'extérieur du swarm si on le souhaite. Il peut assigner automatiquement un "Published Port", un port extérieur, à un service (entre 30 000 et 32 767). On peut aussi choisir nous même un port non utilisé.

## Réseaux overlay
On peut aussi préciser à la création d'un service un réseau overlay. Le but d'un réseau overlay est de permettre la communication entre des containers sur des docker hosts/machines différentes. Tous les containers peuvent participer à un réseau overlay, peu importe si ce sont des tasks définis par un service ou des standalone containers. Dès qu'un container rejoindra le réseau une adresse ip du réseau lui sera attribué. 

### Création d'un réseau overlay
On doit créer le réseau overlay sur un manager. Ce dernier fera savoir aux autres nodes que le réseau existe. 
```
docker network create -d overlay <NETWORK-NAME>
```

Quelques flags à utiliser : 

-  ```--attachable```permets d'attacher manuellement des containers au réseau et donc à des standalone containers d'utiliser le réseau overlay.
- ``` -d / --driver``` indique le driver qui gèrera le réseau (bridge, overlay, ...). Bridge est mis par défaut, il faut donc préciser que c'est un réseau overlay dans notre cas avec ``` --d overlay```.
- ``` --gateway ``` définit une Gateway pour le réseau.
- ```--internal ```empêche l'accès au réseau depuis l'exterieur.
- ```--ipv6 ```active l'IPv6.
- ``` --subnet ``` définit le sous-réseau dans lequel Docker va piocher les adresses à attribuer. Docker attribue par défaut des subnets aux différents réseaux et fait en sorte qu'il ne se superposent pas. Ce flag nous permets de choisir le subnet d'adresses.
- ``` --ip-range ``` permets de définir un sous-ensemble d'adresses ip dans le subnet que Docker utilisera pour l'attributaion des ip.
- etc.

Exemple :
```
 docker network create 
  --d overlay \
  --attachable \
  --subnet=172.28.0.0/16 \
  --ip-range=172.28.5.0/24 \
  --gateway=172.28.5.254 \
  my_overlay
```


Pour plus d'infos ou exemples consultez la documentation [Docker](https://docs.docker.com/engine/reference/commandline/network_create/) ou la commande ``` docker network create --help```.

### Attribuer un réseau overlay à un service

### Attribuer un réseau overlay à un standalone container

### Ports utilisés
