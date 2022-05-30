--------------
Partie Docker
--------------

Installer docker pour ubuntu:

sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt-get update

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

Pour vérifier que tout fonctionne correctement: sudo docker run hello-world

-----------------------------------------------------------------------------

Installer docker-compose :

sudo apt-get update

sudo apt-get install docker-compose-plugin

Pour vérifier que tout fonctionne correctement: docker compose version

-----------------------------------------------------------------------------
Le dossier TP6/ contient un Dockerfile; Aller dans ce dossier et lancer la commande :

Créer l'image jenkins:    docker build -t imagejenkinstp6 .

Pour contruire les différents containers: docker-compose up -d

Pour vérifier qu'ils ont été construits:  docker containers ls
6 containers deveraient être listés: pythonubuntu, pythoncentos, dbubuntu, dbcentos, master01 et jenkins

Réccupérer les adresses IP de tous les containers et les sauver:
docker inspect master01 | grep IPAddress | tail -n 1
docker inspect pythonubuntu | grep IPAddress | tail -n 1
docker inspect pythoncentos | grep IPAddress | tail -n 1
docker inspect dbubuntu | grep IPAddress | tail -n 1
docker inspect dbcentos | grep IPAddress | tail -n 1
docker inspect jenkins | grep IPAddress | tail -n 1

-----------------------------------------------------------------------------
Se connecter au container master01 : docker exec -it master01 "sh"

créer une clé ssh pour relier master01 aux hosts pythonubuntu, pythoncentos, dbubuntu, dbcentos:
ssh-keygen 
accepter le chemin dans lequel la clé sera sauvegardée et stocker le chemin de la clé
définir un mot de passe pour la clé
eval "$(ssh-agent -s)"
ssh-add chemin_de_la_clé

ajouter la clé aux hosts:
ssh-copy-id root@<IP_pythonubuntu>    le mot de passe est "toor"
ssh-copy-id root@<IP_pythoncentos>    le mot de passe est "toor"
ssh-copy-id root@<IP_dbubuntu>    le mot de passe est "toor"
ssh-copy-id root@<IP_dbcentos>    le mot de passe est "toor"

On peut vérifier que la connexion a été faite avec :  ssh root@<Adresse_IP>

sortir de master01 : exit

-----------------------------------------------------------------------------
Se connecter au container jenkins: docker exec -it jenkins "sh"

créer une clé ssh pour relier jenkins à master01:
ssh-keygen 
accepter le chemin dans lequel la clé sera sauvegardée et stocker le chemin de la clé
définir un mot de passe pour la clé
eval "$(ssh-agent -s)"
ssh-add chemin_de_la_clé

ajouter la clé aux hosts:
ssh-copy-id root@<IP_master01>    le mot de passe est "toor"

On peut vérifier que la connexion a été faite avec :  ssh root@<IP_master01>

sortir de jenkins: exit

-----------------------------------------------------------------------------
Se connecter au container master01 : docker exec -it master01 "sh"

Aller dans le dossier "files" : cd files
Lancer le playbook : ansible-playbook.yml playbook.ylm





