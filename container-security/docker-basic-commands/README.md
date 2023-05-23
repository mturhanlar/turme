# Docker Basic Commands

<pre><code>
##Check Docker version  
docker version

##Check host Information  
docker info

##Pull image  
docker pull registry:5000/alpine

##List images

docker images  
docker image ls  

##Run image in background mode  
docker run -dt alpine  

## Run image in interactive mode  
docker run -it alpine

## List running containers  
docker ps  
docker container ls  

## Inspect a container  
docker inspect [Container-id]

## Manage containers  
docker container  

## Manage images  
docker image

## List networks  
docker network ls  


## Manage networks  
docker network

## Attach to a running container  
docker attach [Container-id]  

## Execute a command in a running container  
docker exec -it [Container-id] /bin/sh

## Stop a running container  
docker stop [Container-id]  

## Start a stopped container  
docker start [Container-id]  

## Kill a running container  
docker kill [Container-id]
<strong>
</strong><strong>## Build Docker image  
</strong>
docker build -t alpine-mod

## Push the image  
docker push alpine-mod  

## Commit container as an image

Create a container by running an image. Make some changes to it.  
Here, a text file is created in it. Now, commit it.  

docker commit [Container-id] alpine-note

## Export an image as tar archive  

docker export -o alpine.tar [Container-id]  

##Remove stopped containers
List all running and stopped containers  

​docker ps -a

## Take container IDs of desired containers and delete them.  

docker rm [Container-id1] [Container-id2]  

## Remove an image 

List all images present in the local storage.  

## Remove image by name  
docker rmi -f alpine-note  

## Remove image by Image ID  
docker rmi -f [Container-id]

Check the image list to verify the deletion.

## Remove stopped container, unused images/networks  
​docker system prune -a

</code></pre>

### Creating Docker File and building container from there

```bash
~/test  $ cat Dockerfile

FROM ubuntu:latest

RUN apt-get update && apt-get install -y ssh

```

```
docker build -t ubuntu-test .

# Then run it. 
```
