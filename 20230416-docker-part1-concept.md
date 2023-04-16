---
title: docker core concepts
tags: [docker]
version: 0.1
---
# docker core concepts

## docker host

### image file
image file consist of file system and metadata
- file include language environment and library
- meta data include environment variables, port map and volume

## docker container
image file running in container. Container is instance of image file.
Container can run multiple instances of image file.
container of image file relationship like process of program 
container of image file relationship like instance of class
- run in a start process
- and run another process 
- file change by copy-on-write. the basic image do not change
- container build from image file, and start use metadata to start configuration
- container isolated itself to another container

## docker layer
copy-on-startup vs copy-on-write
just data change to write 
a node.js app layer three
1. todoApp app layer data change 
2. node.js do not change
3. ubuntu  do not change

docker client to guard process 
guard process to docker hub or private registration 
default connect : `/var/run/docker.sock`
access guard process : `tcp://0.0.0.0:2375`

### docker guard process
run as daemon
`docker run -d --restart=always ubuntu echo done`
--echo is simple command

#### docker file move and create 
`docker daemon -g /home/dockeruser/mydocker` --g is start new directory

### docker client
run `docker run` or `docker pull`

#### use `socat` monitor Docker api network use `traffic snopper`
```
sudo socat -v UNIX-LISTEN:/tmp/dockerapi.sock UNIX-CONNECT:/var/run/docker.sock &
```
- -v: verbose 
- UNIX-LISTEN: listen at unix socket
- UNIX-CONNECT: socat connect docker's unix socket
- & run at background

#### export port share container to host
container access from host   
```
docker pull tutum/wordpress
docker run -d -p 10001:80 --name blog1 tutum/wordpress
docker run -d -p 10002:80 --name blog2 tutum/wordpress
```
- -d : daemon
- -p blog1 : host port 10001 map to container name blog1 port
- -p blog2 : host port 10002 map to container name blog2 port

`docker rm -f blog1 blog2` --remove

#### link containers communication
mysql database tier separate from wordpress container
```
docker run --name wp-mysql -e MYSQL_ROOT_PASSWORD=yourSecretPassword -d mysql
docker run --name wordpress --link wp-mysql:mysql -p 10003:80 -d wordpress
``` 
1. --name  naming wp-mysql to mysql's container
2. -e provide environment variable 
3. --link link outside wp-mysql to mysql 
4. -d both run daemon 
5. key is --link make communication between wp-mysql and wordpress
6. Dockfile use `EXPOSE`  

#### use docker in browser 
access docker via REST API 

### docker registration center 
- private registration
- docker hub

#### construct local docker registry
```
docker run -d -p 5000:5000 -v $HOME/registry:/var/lib/registry registry:2
```
use $HOME/registry to /var/lib/registry to store docker image

on the client in guard process
`--insecure-registry HOSTNAME` now can run:
`docker push HOSTNAME:5000/image:tag`

### docker hub

#### search and run docker image
if interest node.js 
`docker search node` and run `docker pull node` to download
interactive run
- -t: tty
- -i interactive
`docker run -t -i node /bin/bash root@c267a2999646` 