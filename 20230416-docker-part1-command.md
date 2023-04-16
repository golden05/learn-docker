---
title: docker command
tags: [docker]
version: 0.1
---
# command run host

- `docker build`  -- construct a docker image
- `docker run`    -- run a docker container
- `docker commit` -- commit a docker image
- `docker tag`   -- tag a docker image

## construct a docker image
- use Dockerfile to build a docker image
- tag a docker image
- run a docker image 

### Dockerfile 
```
FROM node -- define base image
LABEL maintainer="<your email>" 
RUN git clone https://github.com/<your name>/<your repo>
WORKDIR /<your repo>  -- move to my directory
RUN npm install 
EXPOSE 3000
CMD [ "npm", "start" ] 
```

`docker build . `
`docker run -p 5000:3000 --name example1 <your image name>`
5000 is port of container
3000 is port of host in Dockerfile
