![Image](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4e/Docker_%28container_engine%29_logo.svg/langfr-290px-Docker_%28container_engine%29_logo.svg.png)

## 🛠️ Installation  

RHEL/AlmaLinux

```bash
# Adding Docker repository
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# Preparing and installing Docker
dnf remove podman buildah
dnf install docker-ce docker-ce-cli containerd.io

# Enabling Docker service
systemctl start docker.service
systemctl enable docker.service
```

Allow user to run docker command

```bash
$ sudo usermod -aG docker $USER
```

## 🧑🏻‍💻 Useful commands
```bash
# Display docker version information
docker version
docker -v
docker info

# Search container
docker search [name]

# Get container
docker pull hello-world     
docker run hello-world

# List images
docker image ls [OPTIONS] [REPOSITORY[:TAG]]

# Show running containers
docker ps -a 

# run specific container (detach interactive mode)
docker run -di --name container_name alpine:latest

# run specific command inside a container 
docker exec -ti container_name sh

# remove exited containers
docker rm $(docker ps -a -f status=exited -f status=created -q)

# Remove one or more images
docker rmi [OPTIONS] IMAGE [IMAGE...]

# Remove all unused containers, networks, images (both dangling and unreferenced), and optionally, volumes.
docker system prune

```

## Running containers (example with NGINX)
```bash
docker run -tid -p 8080:80 --name web nginx:latest

# --> Connect via web browser 127.0.0.1:8080

docker inspect web
    "IPAddress": "172.17.0.3"
# --> Connect via web browser 172.17.0.3:80

# Start/Stop container
docker start web
docker stop web

# Remove container
docker rm -f web
```

## Volumes

```bash
# Permet de remonter un répertoire pour la machine
docker run -tid -p 8080:80 -v /srv/data/nginx:/usr/share/nginx/html --name web nginx:latest

# Managing volume
docker volume ls
docker volume create monvolume
docker volume inspect monvolume

docker run -tid --name web -p 8088:80 --mount source=monvolume,target=/usr/share/nginx/html nginx:latest

# Suppresion d'un volume
docker volume rm monvolume
docker rm -f web && docker volume rm monvolume
```

## Environment variables

```bash
# Passer une variable
docker run -tid --name testenv --env MYVARIABLE="123456" ubuntu:latest
docker exec -ti testenv sh
$ env

vim vars_env.lst
MYPASSWORD="P@ssword"

docker run -tid --name testenv --env-file vars_env.lst ubuntu:latest

# Changer la variable hostname
docker run -tid --name testenv --hostname ubcontainer ubuntu:latest
```

## Dockerfile

```bash
vim Dockerfile

FROM ubuntu:latest
MAINTAINER pwet
RUN apt-get update \
&& apt-get install -y vim git \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

docker build -t monimage:v1.0 .

docker image ls

# permet de voir l'historique des commandes de l'image
docker history monimage:v1.0

docker run -tid --name ubtest monimage:v1.0
docker exec -ti ubtest sh
$ git

# Suppression de l'image
docker rmi monimage:v1.0
```

## Docker layers

2 types de couches : ro et rw
les couches peuvent etre partagées
