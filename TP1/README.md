### TP 1 Docker

## database

# create network
docker network create TP1

# run image database
docker run --network=TP1 --name database -P database

# Run image adminer
docker run --name adminer --network=TP1 -p 8090:8080 adminer
```

## backend API

# Build the dockerfile dans le répertoire courant
dockerfile build -t simpleapi .

# create network 
docker network create TP1

# run l'image simpleapi 
docker run -d --name=simpleapi --network=TP1 -p 8080:8080 simpleapi

# Build dockerfile backendapi dans le repertoire simple-api-student-main
docker build -t backendapi .

# Run l'image backendapi
docker run -d --network=TP1 --name=backendapi -p 8082:8080 backendapi

# build docker-compose
docker-compose up --build

## http server

# build dockerfile dans web/simple-html
docker build -t  httpserver .

# run l'image httpserver :
docker run --name=httpserver -p 8080:80 httpserver

## Docker-compose

# run le docker-compose
docker-compose run --build

## Docker hub

# login to docker 
docker login

# tag the database to docker hub
docker tag tp1_database evesquemathieu/devopstpdocker:tp1_database

# push l'image sur docker hub
docker push evesquematthieu/devopstpdocker:tp1_database

# meme chose pour httpd et api


### TP2 CI/DI and Gitub action

## First steps into the CI world

# What are testcontainers ?

Testcontainers est une bibliothèque Java qui prend en charge les tests JUnit, en fournissant des instances docker légères.

Les Testcontainers facilitent les types de tests suivants :
  - Tests d'intégration de la couche d'accès aux données
  - Tests d'intégration d'applications
  - Tests d'interface utilisateur/de réception
  - Et bien plus encore...

# Creation and configuration of workflows
See my [main.yml workflow file](./.github/workflows/.main.yml)

Successful run testbackend :

# creation secret docker hub
création des tokens "DOCKERHUB_USERNAME" dockerhub et lien sur github : voir images readme.

# validation workflow test-backend et build-and-push-docker-image
voir image readme.


### Setup Quality Gate

# Create account on SonarCloud
# Create new Organisation & project
project key:evesquematthieu_evesquematthieu
organisation key : evesquematthieu

# add sonercloud token
prendre le SONAR_TOKEN de sonarcloud de ton projet et le mettre sur github.

# ajouter le login sonar dans le main.yml
voir image readme --> sonarcloud passed !


### TP 3 ANSIBLE








