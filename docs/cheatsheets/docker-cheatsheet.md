---
layout: default
title: Docker cheatsheet
parent: Cheatsheets
nav_order: 1
---
# Docker cheatsheet
{: .no_toc }
This cheat sheet features the most important and commonly used docker commands for easy reference.
Docker Tips and Tricks (A collection of useful tips and tricks for Docker)


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## [Docker Commands](https://docs.docker.com/engine/reference/commandline/docker/#child-commands)

## [The Ultimate Docker Cheat Sheet](https://dockerlabs.collabnix.com/docker/cheatsheet/?utm_source=pocket_mylist)

## Stop all containers
**NOTE:** This will stop ALL your containers.
```sh
docker stop $(docker ps -aq)
```

## Remove all containers
**NOTE:** This will remove ALL your containers.
```sh
docker rm $(docker ps -a -q)
```

## Stop and remove all images and containers
**NOTE:** This will remove ALL your images/containers.
```sh
docker stop $(docker ps -aq) && docker rm $(docker ps -aq) && docker rmi $(docker images -q)
```

## Remove all images
**NOTE:** This will remove ALL your images.
```sh
docker rmi $(docker images -q)
```

## Remove all untagged containers
```sh
docker rmi $(docker images | grep '^<none>' | awk '{print $3}')
```

## Connect to a docker container
```sh
sudo docker exec -i -t [CONTAINER ID] /bin/bash
```

## See all space Docker take up
```sh
docker system df
```

## Get IP address of running container
```sh
docker inspect [CONTAINER ID] | grep -wm1 IPAddress | cut -d '"' -f 4
```

## Kill all running containers
```sh
docker kill $(docker ps -q)
```
## Removing All Unused Objects
```sh
$ docker system prune
$ docker system prune --volumes
$ docker network prune #Remove all unused networks
```
