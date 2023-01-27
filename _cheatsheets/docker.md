---
layout: page
title: docker
description: some useful ways to use docker.
img: 
importance: 2
category: tools
---
## General Purpose

~~~ shell
#
# lists all images
docker image ls		
docker images
~~~
~~~ shell
#
# removes an image
docker image rm <image>		
docker rmi <image>
~~~
~~~ shell
#
# pulls image from a docker registry
docker image pull <image>		
docker pull
~~~
~~~ shell
#
# lists all containers
docker container ls -a		
docker ps -a
~~~
~~~ shell
#
# runs a container from an image
docker container run <image>	
docker run
~~~
~~~ shell
#
# removes a container 
docker container rm <container>	
docker rm
~~~
~~~ shell
#
# stops a container
docker container stop <container>		
docker stop
~~~
~~~ shell
#
# executes a command inside the container
docker container exec <container>	 	
docker exec
~~~