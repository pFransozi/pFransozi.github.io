---
layout: post
title:  docker - cheatsheet
date:   2023-01-28 01:00:00
description: some useful ways to use docker.
language: en-us
tags: ["cheatsheet", "tools", "docker"]
language: en-us
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

## create a docker group and add it to user group
~~~ shell
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
~~~

## create a docker container from mongo image
~~~ shell
docker run --name mymongo -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=PUT-USERNAME-HERE \
            -e MONGO_INITDB_ROOT_PASSWORD=PUT-PASSWORD-HERE mongo
~~~

## create a docker container from mssql image
~~~ shell
sudo docker pull mcr.microsoft.com/mssql/server:2022-latest

sudo docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=PUT-PASSWORD-HERE" -p 1433:1433 \
                --name mymssql --hostname mymssql -d mcr.microsoft.com/mssql/server:2022-latest

sudo docker exec -it sql1 "bash"
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "PUT-PASSWORD-HERE"
~~~

## create a docker container from mysql image
~~~ shell
# a exclusive volume: -v mysql:/var/lib/mysql/ 
# to connect from a container is recommended create a network:
## docker network create my-sql-network
## --network my-sql-network
docker run --name mysql -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=PUT-PASSWORD mysql:latest
docker logs mysql --follow
docker exec -it mysql mysql -p
~~~

## create a docker container from postgres image
~~~ shell
#docker pull Postgresql
#docker pull dpage/pgadmin4 #web-based administration tool for PostgreSQL

#docker network create --driver bridge postgres-network
# for more information about postgres environment variables, see here: hub.docker.com/_/postgres
# POSTGRES_PASSWORD env variable uses the default user, which is postgres
#docker run --name mypostgres     --network=postgres-network -e "POSTGRES_PASSWORD=[ENTER PASSWORD HERE]" -p 5432:5432 -d postgres
docker run --name mypostgres -e "POSTGRES_PASSWORD=ENTER-PASSWORD-HERE" -p 5432:5432 -d postgres
#docker ps

#docker run --name postgres-admin --network=postgres-network -p 15432:80 -e "PGADMIN_DEFAULT_EMAIL=[ENTER AN EMAIL HERE]" -e "PGADMIN_DEFAULT_PASSWORD=[ENTER POSTGRES SERVER PASSWORD HERE]" -d dpage/pgadmin4
~~~