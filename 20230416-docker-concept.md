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