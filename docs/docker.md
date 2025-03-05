# Docker

## Build de l'image Docker

un Dockerfile est présent dans le directory _/docker_.
Pour builder l'image Docker, il faut lancer la commande _docker build_ depuis la racine du projet:

```
docker build -f docker/Dockerfile -t rdv-service-public:latest .
```

L'image docker va ici se nommer _rdv-service-public_ et être créée avec le tag _latest_.

## Fichier compose

Un fichier compose _rdv-service-public.yml_ est également présent dans le directory _/docker_. Il contient les services nécessaires au bon fonctionnement de _rdv-service-public_, à savoir un service PostgreSQL et un service Redis.


### Configuration du service

Le service se configure dans le fichier _.env_, qui doit être présent dans le directory docker. S'il n'existe pas, recopier _.env.sample_ dans _docker/.env_.

Les variables _POSTGRE\_*_ suivantes doivent être configurées:
```
POSTGRES_HOST
POSTGRES_USER
POSTGRES_PASSWORD
```

#### POSTGRES_HOST

Si vous ne modifiez pas le fichier de composition Docker, vous pouvez renseigner POSTGRES_HOST ainsi `POSTGRES_HOST=rdv-service-public-db-1`. En effet un réseau virtuel Docker est créé avec la composition Docker, et une résolution de nom est faite à l'intérieur de ce réseau. Les noms des hosts sont dérivés du nom du projet et des noms des containers, par défaut le host correspondant au service PostgreSQL se nomme donc _rdv-service-public-db-1_

#### POSTGRES_USER et POSTGRES_PASSWORD

Pour POSTGRES_USER il faut renseigner un user avec les droits super_admin. Par défaut l'image docker postgresql contient le user _postgres_, vous pouvez donc l'utiliser ainsi `POSTGRES_USER=postgres`
Le POSTGRES_PASSWORD doit également être renseigné. Le mot de passe renseigné va définir le mot de passe du user defini dans POSTGRES_USER.

#### REDIS

Par defaut la variable REDIS_URL pointe sur localhost. Localhost ne va pas fonctionner dans le réseau Docker. Il faut donc définir REDIS_URL ainsi (l'a rajouter dans _.env_): `REDIS_URL=redis://rdv-service-public-redis-1:6379`

#### Skip Scalingo

Une variable liée à scalingo doit être renseigné pour éviter la configuration du service Scalingo qui ne nous intéresse pas ici `SECRET_KEY_BASE_DUMMY=1`


### Démarrage de la composition

La composition Docker peut être démarrée ainsi, depuis la racine du projet:

```
docker compose -f docker/rdv-service-public.yml --env-file ./docker/.env up
```

Une fois le démarrage fait, le service _rdv-service-public_ est disponible sur _http://localhost:3000_
