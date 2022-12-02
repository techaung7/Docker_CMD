# Docker Installation on Ubuntu Linux By TECHAUNG
# Install using the repository

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    Lsb-release

sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

# Install Docker Engine
```bash
sudo apt-get update

#If there is error type 
sudo chmod a+r /etc/apt/keyrings/docker.gpg
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo docker run hello-world
```

# UnInstall Docker Engine

```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
 
# Post-installation steps for Linux

Manage Docker as a non-root user
```bash
sudo groupadd docker
sudo chmod -aG docker $USER
newgrp docker

#Configure Docker to start on boot with systemd
sudo systemctl enable docker.service
sudo systemctl enable containerd.service

sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```
# Docker Images 
```bash
docker images

docker pull repository:version     #pull image from docker hub 
docker pull ubuntu:latest           

docker inspect ubuntu:latest       #show stats of images

docker rmi <images ID>             #delete image
docker images -q                   #show images only ID
docker rmi $(docker images -q) -f  #delete all images 
```
# Docker Help 
```bash    
docker run - -help 
docker - -version
```
# Docker Containers
```bash
docker run -it repository:version                  #run container with interattch mode
docker run -it repository:version whoami           #direct command to container 
docker run --name HtetAung -it repository:version  #name to container & run 

docker run -it --name HtetAung -p80:80 -p143:143 repository:version       #port open & run 

docker run -d -it repository:version                #background run    
docker exec -it <ID> bash                           #relogin to container
docker stats <ID>                                   #show info of containers
docker stop repository:version 
docker rm repository
docker ps                                           #show running containers
docker ps -a                                        #show all containers

#Docker Commit
docker commit -a htetaung -m “Message” <ID>  <imagename>     #container to images 
```

# DockerFile (Useful Dockerfile Commands)

FROM     
RUN 
CMD
ENTRYPOINT
ADD
COPY
ENV
WORKDIR
MAINTAINER
USER
VOLUME


```bash
docker build -t  <name> .             #Build Image with Dockerfile
```

# Docker Push
```bash
docker login     
docker tag <imageID>  username/imagename
docker push username/imagename
```
# Docker Nerwork

```bash
#Bridge Network 
docker nerwork create <Network Name>
docker network ls 
docker network connect <Network Name> <Container Name>
docker network disconnect <Network Name> <Container Name>
docker network rm <Network Name> 

#Host Network (Within A Host)
docker run --rm -d --network host --name <my_nginx> <nginx>
ip a|grep ens                 #Check IP of host

#Overlay Network (HOST A to Host B)
docker swarm init          #initialize docker swarm /wll get tocken 
docker network create --driver=overlay --attachable <NetworkName>
docker swarm join --tocken <TOEKN ID>
docker swarm leave --force
```
# Docker Swarm 
```bash
docker swarm init                              #Manager 
docker swarm join --tocken <TOEKN ID>          #Join As Worker
docker swarm leave --force
docker swarm init --advertise-addr <IP>:2377 --listen-addr <IP>:2377
docker node ls
docker swarm join-token worker          #Show Worker Token
docker swarm join-token manager         #Show Manager Token
docker service create --name <NAME> -p 8080:8080 --replicas 5 <Image Name>
docker service ls 
docker service ps <NAME>

```
