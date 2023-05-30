# student-list 

Ce repository est une simple application qui permet d'afficher une liste d'étudent de a l'aide d'une API et un webserver PHP
![project](https://user-images.githubusercontent.com/18481009/84582395-ba230b00-adeb-11ea-9453-22ed1be7e268.jpg)


------------


## creation d'une image docker pour notre API aveec un test 

Ce repository est une simple application qui permet d'afficher une liste d'étudiant à l'aide d'une API et un webserver PHP
La création de notre API se fait par les étapes suivantes : 

-  création d'un fichier ***Dockerfile*** dans le dossier simple api et respecter les étapes de build indiqué dans le repository suivant : [here](https://github.com/diranetafen/student-list.git "here")
- la création de l'image avec la ligne de commande suivante dans le terminal (machine sous CentOS7 avec docker déjà installé), à ne pas oublier que la commande doit être exécutée dans le répertoire du ficher Dockerfile : 

```
docker build -t pozos:v1 .
```
- démarrer le conteneur de l'application après la création de l'image (POZOS) (à ne pas oublier de monter le volume qui contient les fichiers de l'API dans le dossier spécifié par les développeurs) : 

```
docker run --name pozos -d -p 80:5000 -v ./:/data/ pozos:v1
```
- tester le fonctionnement de l'application avec la commande suivante (host API est indiqué sur  3: enp0s8 en utilisant la commande ` ip a;`):

```
curl -u toto:python -X GET http://<host IP>:<API exposed port>/pozos/api/v1.0/get_student_ages
```
- le résultat de cette ligne de commande doit être cette liste : 
```
{
  "student_ages": {
    "alice": "12", 
    "bob": "13"
  }
}
```

- supprimer le conteneur après le bon fonctionnement du test avec la commande suivante : 
```
docker ps  #pour récupérer ID du conteneur
docker rm <ID conteneur> 
```

### création de l'infrastructure a l'aide de docker compose

Dans cette partie de ce projet, nous allons créer le site qui permettre d'afficher la liste d’étudiant, le site est tout simplement un conteneur crée à la base d'une image PHP qui permettre de lire le ficher PHP crée par le développeur de l'application et afficher la liste des étudiant, nous allons suivre les étapes suivantes : 

- création du ficher ***docker-compose.yml*** :
```
vi docker-compose.yml
```
Dans notre cas, pozos représente l’API, et PHP représente le website

POZOS : ne pas oublier à monter le volume qui contient les ficher de l’API.
PHP : le username est toto , password est python 

- avant de lancer le docker compose ne pas oublier de modifier la ligne 29 dans le ficher sutdent-list/website/index.php avec les bon paramètres (HOST API, Port), exécuter le docker compose à l'aide de la ligne de commande suivante : 

```
Docker compose up -d
```

- vérifier que le site est fonctionnel, et le résultat doit être comme suit : 
![22](https://github.com/adda213/mini-projet-docker/assets/123883398/465d8afa-c04d-41c4-bf97-54309c7b5fb4)

