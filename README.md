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

# copier la clé privé dans ~/.ssh 
cp /home/tp/Téléchargements/id_rsa ~/.ssh

# augmenter les permissions de sa clé : 
chmod 400 ~/.ssh/id_rsa

# se connecter en ssh au serveur takima : 
ssh -i ~/.ssh/id_rsa centos@matthieu.evesque.takima.cloud

# ajouter notre hote sur etc/ansible/hosts
vim /etc:ansible/hosts 
--> ajouter centos@matthieu.evesque.takima.cloud

# /!\ problèmes de droit , impossible de modifier le fichier !
sudo chmod 777 /etc/ansible/hosts

# ping le serveur centos : 
ansible all -m ping --private-key=~/.ssh/id_rsa -u centos

## Setup apache server

# set apache avec ansible:
ansible all -m yum -a "name=httpd state=present" --private-key=~/.ssh/id_rsa -u centos --become   
  --> c'est bon !

# lancer apache sur ansible : 
ansible all -m service -a "name=httpd state=started" --private-key=~/.ssh/id_rsa -u centos --become

le serveur est en ligne ! aller sur matthieu.evesque.takima.cloud

# créer le setup.yml et le ping :
'''
all:
 vars:
   ansible_user: centos
   ansible_ssh_private_key_file: ~/.ssh/id_rsa
 children:
   prod:
     hosts: centos@matthieu.evesque.takima.cloud
'''

ansible all -i inventories/setup.yml -m ping

# demander au serveur notre OS: 
ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"

--> OS = CentOS 

# remove le httpd Apache : 
ansible all -i inventories/setup.yml -m yum -a "name=httpd state=absent" --become

--> le serveur n'est plus accésible sur le navigateur

## Playbooks

# créer son premier playbook.yml : 
'''
- hosts: all
  gather_facts: false
  become: yes

  tasks:
   - name: Test connection
     ping:
'''

# executer le playbook:
ansible-playbook -i inventories/setup.yml playbook.yml

--> OK

# création du playbook avancé :
## une fois le playbook créé, se reconnecter en ssh à centOS et vérifier que docker est bien installé.

### run le playbook
ansible-playbook -i inventories/setup.yml playbook.yml

### se connecter en ssh au serveur takima : 
ssh -i ~/.ssh/id_rsa centos@matthieu.evesque.takima.cloud

### une fois connecté un test la commande "docker" et il reconnait la commande --> docker est bien installé !

## utiliser les roles

# initialiser les roles et y mettre la docker task
ansible-galaxy init roles/docker

# faire la même chose pour les autres roles : 
ansible-galaxy init roles/network
ansible-galaxy init roles/database
ansible-galaxy init roles/app
ansible-galaxy init roles/proxy

# mettre dans le tasks/main.yml la ache de création correspondante.
# mettre à jour le playbook pour lancer les différentes roles : 
'''
 - hosts: all
  gather_facts: false
  roles:
    - docker #lunch task docker
    - network #lunch task network
    - database #lunch task database
    - api #lunch task api
    - httpd #lunch task httpd
'''







