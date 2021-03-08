# challenge07

Pour la solution 1 - Build a new Jenkins image with Docker installed, j'ai utilisé le Dockerfile du dossier Solution 1 et puis j'ai exécuté les commandes suivantes:
docker build --tag jenkins-docker .

docker run --rm -d -v .//jenkins_home://var//jenkins_home -p 8082:8080 -p 50001:50000 --privileged jenkins-docker

qui m'ont donné ce résultat:
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                                              NAMES
4cb7ae7d6b5f   jenkins-docker   "/bin/sh -c 'service…"   50 seconds ago   Up 50 seconds   0.0.0.0:8082->8080/tcp, 0.0.0.0:50001->50000/tcp   gifted_shockley

Pour la Solution 2 - Mount the host's docker unix socket onto the Jenkins container, j'ai utilisé le docker-compose.yml du dossier Solution 2 puis j'ai exécuté la commande
docker-compose up


Pour la Solution 3 - Run the official docker-in-docker image and expose its TCP socket to the Jenkins container, j'ai utilisé le docker-compose.yml du dossier Solution 3 puis j'ai exécuté la commande:
docker-compose up

Pour le HOW-TO officiel d'installation de Jenkins sous Docker, qui correspond à la Solution 3 de Tiu Wee Han, j'ai utilisé le Dockerfile du dossier jenkins-officiel puis j'ai exécuté les commandes suivantes:

docker network create jenkins

docker run --name jenkins-docker --rm --detach --privileged --network jenkins --network-alias docker --env DOCKER_TLS_CERTDIR=/certs --volume jenkins-docker-certs:/certs/client 
--volume jenkins-data:/var/jenkins_home docker:dind

qui m'ont donné ce résultat:
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS          PORTS           NAMES
5b0e149175e9   docker:dind   "dockerd-entrypoint.…"   44 seconds ago   Up 44 seconds   2375-2376/tcp   jenkins-docker

Ensuite j'ai fait:
docker build --tag jenkins-officiel-docker .

docker run --name jenkins-blueocean --rm --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro --publish 8085:8080 --publish 50005:50000 jenkins-officiel-docker

Et comme tout était en ordre, j'ai pu aller sur le Setup wizard de Jenkins.
