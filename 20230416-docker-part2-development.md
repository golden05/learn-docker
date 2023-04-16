---
title: docker development
tags: [docker]
version: 0.1
---
# use docker as light VM

## from VM to Container

### convert VM to Container
vm : local and remote
local VirtualBox  .vdi or .vmdk 

### like host container
assume container as a host can run multi process
```
docker run -d phusion/baseimage
docker exec -i -t 3c333 /bin/bash
```
1. run as daemon
2. run start command /bin/bash and return container id 
3. in the container start a new process /bin/bash

### separate system as some micron service containers
one container one responsibility
use docker make app and based container service
postgresql, nodejs and nginx app
```
FROM ubuntu
RUN apt install postgresql nodejs npm nginx
WORKDIR /opt
COPY db /opt/db
RUN service postgresql start && \
    cat db/schema.sql | psql && \
    service postgresql stop
COPY app /opt/app
RUN cd app && npm install 
RUN cd app && ./minift_static.sh
COPY conf /opt/conf
RUN cp conf/mysite /etc/nginx/sites-available/ && \
    cd /etc/nginx/sites-enabled && \
    ln -s ../sites-available/mysite
``` 

make three Dockerfile 
1. database Dockerfile
```
FROM ubuntu:14.04
RUN apt-get update && apt-get install -y postgresql
WORKDIR /opt
COPY db /opt/db
RUN service postgresql start && \
    cat db/schema.sql | psql && \
    service postgresql stop
```
2. nodejs Dockerfile
```
FROM ubuntu:14.04
RUN apt-get update && apt-get install -y nodejs npm
WORKDIR /opt
COPY app/package.json /opt/app/package.json
RUN cd app && npm install
COPY app /opt/app
RUN cd app && ./minift_static.sh 
```
3. web server Dockerfile
```
FROM ubuntu:14.04
RUN apt-get update && apt-get install -y nginx
WORKDIR /opt
COPY conf /opt/conf
RUN cp conf/mysite /etc/nginx/sites-available/ && \
    cd /etc/nginx/sites-enabled && \
    ln -s ../sites-available/mysite
```

## manage container service 
has not init process 
init process run id #1 based parent process id #0

### manage container service start 
```
FROM ubuntu:14.04
RUN apt-get update && apt-get install -y python-pip apache2 tomcat7
ENV DEBIAN_FRONTEND noninteractive
RUN pip install supervisor
RUN mkdir -p /var/lock/apache2
RUN mkdir -p /var/run/apache2
RUN mkdir -p /var/log/tomcat7
RUN echo_supervisord_conf > /etc/supervisord.conf
ADD ./supervisord_add.conf /tmp/supervisord_add.conf
RUN cat /tmp/supervisord_add.conf >> /etc/
```

### tag docker 
`docker tag ddddd imageName`
`docker run imageName`

### share at docker hub
assume user at hub is adev
```
docker pull debian:wheezy
docker tag debian:wheezy adev/debian:mywheezy
docker push adev/debian:mywheezy
```

## process is environment 

# docker day after day 

## volume persistent 

### docker volume persistent
volume as docker life cycle  that in a container access file outside
host file path map container file
/var/db/tables mount as /var/data1 in the container
`docker run -v /var/db/tables:/var/data1 it debian bash`
-v volume point outside volume

### BitTorrent Sync distribute volume
share volume on multi host on www
- BTSync server is a docker container 
- BTSync client own local data volume
- client mount by BTSync client 
1. on the BTSync server
```
docker run -d -p 8888:8888 -p 55555:55555 --name btsync ctlc/btsync
docker logs btsync

```

### kill stop docker

## construct image

### add command 

