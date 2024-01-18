# Docker-Course

I will be sharing the short notes that I have made while learning on how to containerize any application using Docker:
***wsl --install -d Ubuntu    ***docker --version
-DOCKER - It's an open-source containerization platform that allows you to containerize 
your applications, share them using public or private registries, and also to orchestrate them.

-wsl --install -d Ubuntu   wsl --set-default-version 2

-Containerization involves encapsulating or packaging up software code and all its 
dependencies so that it can run uniformly and consistently on any infrastructure.(Own isolated environment)
-Container us running environment for image
-Multiple containers can run on your computer

Docker file - A blueprint for building images ---The name MUST be "Dockerfile"
Syntax:
FROM "name"
ENV - Environment variables
RUN mkdir -p/home/app
COPY ./home/app -copy current folder files to /home/app(Executes on the HOST machine)
CMD["node","server.js"] - Start the app "node server.js" CMD is an entry point command

Images are multi-layered self-contained files that act as the template for creating containers
An image registry is a centralized place where you can upload your images and can also download images created by others. 
Docker Hub is the default public registry for Docker

-Docker image is the actual package(artifact that can be moved around) Docker container is a running environment for IMAGE
do
**DOCKER COMMANDS***
docker ps = list running containers docker ps -a = list running and stopped containers
docker pull =Downloads the docker image to your local machine from docker private repo
docker run = Creates a new container(Combines docker pull and docker start) -d=detach mode with specific version
docker run -d redis:version = Pulls the images and starts the container
docker stop id = starts the container  docker start id = Starts the stopped container
docker prune = removes all the stopped containers
docker run -p6000:6379 application -p6000=host port 6379=Container port
docker run -p 5173:5173 -v "%cd%:/app" -v /app/node_modules react-docker (WINDOWS)
docker run -p 5173:5173 -v "$(pwd):/app" -v /app/node_modules react-docker(Linux/mac)
docker tag react-docker snex001/react-docker(Publish docker image)
docker push snex001/react-docker
docker run -d -p6000:6379 --name redis-older redis:4.0 = Named our container "redis-older"
docker logs id/name = Gives the logs of your container
docker system prune -a = clean up any resources — images, containers, volumes, and networks
docker exec -it id /bin/bash = Moves to the tab the given container is running /Run a command in a running container
rm - Removes one or more containers
rmi -Remove one or more images
build       Build an image from a Dockerfile  docker build -t my-app:1.0 . Jenkins build the Docker image from the dockerfile
update - Update configuration of one or more containers
docker network create name = Creates a new network  docker network connect "network name" "container name"
docker container rename <container identifier> <new name>
docker run -v data_volume:/var/lib/mysql mysql ---> Volume mounting
docker run-v /data/mysql:/var/lib/mysql mysql ---> Bind mounting
docker -H=10.123.2.1:2375 run nginx
docker run -d --network some-network --name some-mongo \
	--subnet 182.18.0.0/16
	-p 27017:27017
	-e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
	-e MONGO_INITDB_ROOT_PASSWORD=secret \
	mongo
docker-compose -f mongo.yaml up -d = Starts the containers in yamls file. down=stops them
NB: When you restart a container, everything that you had configured will be gone.
Docker volumes - Persistent data in docker Docker Volume Locations:
docker run --name website -v $(pwd):/usr/share/nginx/html:ro -d -p 8080:80 nginx
Win = C:\ProgramData\docker\volumes Linux&Mac=/var/lib/docker/volumes
docker push AWSname:tag
docker scan imaageName
Image Naming in Docker registries---> registryDomain/imageName:tag
docker tag "from" "destination" --> docker tag Grayscale-website:latest Grayscale-website:1

SET FORMAT= "ID\t{{.ID}}\nNAME\t{{.Names}}\nIMAGE\t{{.Image}}\nPORTS\t{{.Ports}}\nCOMMAND\t{{.Command}}\nCREATED\t{{.CreatedAt}}\nSTATUS\t{{.Status}}\n"

JENKINS IN DOCKER:
docker run -p 8080:8080 -p 50000:50000 -d -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts

SHARING THE IMAGE ON DOCKERHUB:
docker image build --tag snex001/custom-nginx:latest --file Dockerfile.built .
docker image push snex001/custom-nginx:latest

docker volume inspect myvolume

docker container run --rm --detach --publish 3000:3000 --name hello-dock-dev --volume $(pwd):/home/node/app --volume /home/node/app/node_modules hello-dock:dev

docker container run --rm --detach --name hello-dock-prod --publish 8080:80 hello-dock:prod

docker container inspect --format='{{range .NetworkSettings.Networks}} {{.IPAddress}} {{end}}' notes-api-db-server

docker network inspect --format='{{range .Containers}}{{.Name}}{{end}}' bridge

docker container exec notes-api npm run db:migrate = Executing a command inside a running container

docker container run --detach --volume notes-db-data:/var/lib/postgresql/data --name=notes-db --env POSTGRES_DB=notesdb --env POSTGRES_PASSWORD=secret --network=notes-api-network postgres:12

You can set the logging driver for a specific container by using the --log-driver flag to docker container create or docker run:
 docker run \
      --log-driver json-file --log-opt max-size=10m \
      alpine echo hello world
docker run -it --log-opt max-size=10m --log-opt max-file=3 alpine ash

Docker Swarm is a feature of Docker that makes it easy to run Docker hosts and containers at scale
The network ports required for a Docker Swarm to function correctly are:
*TCP port 2376 for secure Docker client communication. This port is required for Docker Machine to work. Docker Machine is used to orchestrate Docker hosts.
*TCP port 2377. This port is used for communication between the nodes of a Docker Swarm or cluster. It only needs to be opened on manager nodes.
*TCP and UDP port 7946 for communication among nodes (container network discovery).
*UDP port 4789 for overlay network traffic (container ingress networking).

To backup Docker Enterprise Edition you need to create individual backups for each of the following components:
-Docker Swarm. Backup Swarm resources like service and network definitions.
-Universal Control Plane (UCP). Backup UCP configurations.
-Docker Trusted Registry (DTR). Backup DTR configurations and images.
