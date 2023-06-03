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
- démarrer le conteneur qui contient l'application après la création de l'image (POZOS) (à ne pas oublier de monter le volume qui contient les fichiers de l'API dans le dossier spécifié par les développeurs) : 

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

![image](https://github.com/adda213/mini-projet-docker/assets/123883398/cfce9f0f-9dda-4098-88ef-66d856c5c7eb)


PHP : le username est toto , password est python 

![image](https://github.com/adda213/mini-projet-docker/assets/123883398/f2c2c8ff-d576-43d8-b364-20a13bf4f1cf)


- avant de lancer le docker compose ne pas oublier de modifier la ligne 29 dans le ficher sutdent-list/website/index.php avec les bon paramètres (HOST API, Port), exécutez le docker compose à l'aide de la ligne de commande suivante : 

```
Docker compose up -d
```

- vérifier que le site est fonctionnel, et le résultat doit être comme suit : 
![22](https://github.com/adda213/mini-projet-docker/assets/123883398/465d8afa-c04d-41c4-bf97-54309c7b5fb4)


## CREATION D'UN REGISTRY LOCAL 

Apres la creation et le test de notre application , nous allons stocké notre image que nous avons testé dans un  registry et afficher a l'aide d'une interface d'usage toutes les images que nous avons stocké .

![image](https://github.com/adda213/mini-projet-docker/assets/123883398/8289f5bc-2ade-4ed5-a9aa-cab40d9ea24f)

- image : represente l'image de l'application et la version de REGISTRY utilisé pour creer le conteneur registry, 
- volume : reprsente le volume monté dans le conteur qui permettre le stockage des images localement .
- ports : 5000 a gauche represente le ports externe , 5000 a droite reprsesente le port interne
- network : represente le reseau qui contient notre conteneur , ce reseau est ajouté afin de créer une comunnication interne entre le registry , et l'interface d'usage .

## CREATION D'UNE INTERFACE D'USAGE

cette interface va nous permettre de nous afficher toute images sotcké dans le registry local, et permettre aussi de gerer les images par l'ajout des variables d'environnement qui reste optionnel dans ce projet . 

![image](https://github.com/adda213/mini-projet-docker/assets/123883398/e0cbb06a-2160-4ab8-a76e-ffaa57d135e1)

- image : represente l'image de l'application et la version de REGISTRY_UI utilisé pour creer le conteneur .
- ports : 4000 a gauche represente le ports externe , 80 a droite reprsesente le port interne
- environnement : reprsente les variable d'environement insipensable et optionnel 
    REGISTRY_TITLE : le titre du REGISTRY_UI
    NGINX_PROXY_PASS_URL :Mettez à jour la configuration Nginx par défaut et définissez le proxy_pass sur votre registre backend      docker
    SINGLE_REGISTRY : permettre d'autoriser ou non de changer dynamiquement le REGISTRY URL ( en cas plusieurs registry)
    SHOW_CONTENT_DIGEST : permet de voir le detail du docker tags 
    
apre la modification du ficher docker compose , il faut le relancer en utilisant la commande suivante : 

```
Docker compose up -d
```
creer un nouveau tags pour l'image pozos : 
```
docker tag pozos:v1 localhost:5000/pozos:v1
```
pousser l'application dans le registry local : 
```
docker push localhost:5000/pozos:v1
```
![image](https://github.com/adda213/mini-projet-docker/assets/123883398/df8c73df-ea56-4aae-bdba-49fe5feaa9f4)

verifier que le REGISTRY_UI et fonctionnel , et que notre image est bien affiché dans le registry_ui :

![image](https://github.com/adda213/mini-projet-docker/assets/123883398/ba8f6c09-fd9c-458d-b30d-6932ea10b211)

FIN.





    

