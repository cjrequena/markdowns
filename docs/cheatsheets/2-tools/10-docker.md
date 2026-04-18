---
layout: default
title: docker
parent: tools
grand_parent: cheatsheets
permalink: /docs/tools/docker
nav_order: 10
---
# Docker cheatsheet
{: .no_toc }
This cheat sheet features the most important and commonly used Docker commands for easy reference.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## References

- [Docker CLI Reference](https://docs.docker.com/reference/cli/docker/)
- [The Ultimate Docker Cheat Sheet](https://dockerlabs.collabnix.com/docker/cheatsheet/)

---

## Images

### Build an image
```sh
docker build -t <image_name>:<tag> .
```

### Build with a specific Dockerfile
```sh
docker build -f <Dockerfile> -t <image_name>:<tag> .
```

### List images
```sh
docker images
```

### Remove an image
```sh
docker rmi <image_id>
```

### Remove all images
```sh
docker rmi $(docker images -q)
```

### Remove all untagged images
```sh
docker rmi $(docker images | grep '^<none>' | awk '{print $3}')
```

### Pull an image
```sh
docker pull <image_name>:<tag>
```

### Push an image
```sh
docker push <image_name>:<tag>
```

### Tag an image
```sh
docker tag <image_id> <image_name>:<tag>
```

---

## Containers

### Run a container
```sh
docker run -d --name <container_name> -p <host_port>:<container_port> <image_name>:<tag>
```

### Run a container with environment variables
```sh
docker run -d --name <container_name> -e KEY=VALUE <image_name>:<tag>
```

### Run a container with a volume mount
```sh
docker run -d --name <container_name> -v <host_path>:<container_path> <image_name>:<tag>
```

### List running containers
```sh
docker ps
```

### List all containers
```sh
docker ps -a
```

### Stop a container
```sh
docker stop <container_id>
```

### Stop all containers
**NOTE:** This will stop ALL your containers.
```sh
docker stop $(docker ps -aq)
```

### Start a container
```sh
docker start <container_id>
```

### Restart a container
```sh
docker restart <container_id>
```

### Remove a container
```sh
docker rm <container_id>
```

### Remove all containers
**NOTE:** This will remove ALL your containers.
```sh
docker rm $(docker ps -aq)
```

### Stop and remove all containers and images
**NOTE:** This will remove ALL your images and containers.
```sh
docker stop $(docker ps -aq) && docker rm $(docker ps -aq) && docker rmi $(docker images -q)
```

### Kill all running containers
```sh
docker kill $(docker ps -q)
```

### Connect to a running container
```sh
docker exec -it <container_id> /bin/bash
```

### View container logs
```sh
docker logs <container_id>
```

### Follow container logs
```sh
docker logs -f <container_id>
```

### Copy files from container to host
```sh
docker cp <container_id>:<container_path> <host_path>
```

### Copy files from host to container
```sh
docker cp <host_path> <container_id>:<container_path>
```

---

## Inspect

### Get IP address of a running container
```sh
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_id>
```

### See all space Docker takes up
```sh
docker system df
```

### Inspect a container
```sh
docker inspect <container_id>
```

---

## Volumes

### Create a volume
```sh
docker volume create <volume_name>
```

### List volumes
```sh
docker volume ls
```

### Remove a volume
```sh
docker volume rm <volume_name>
```

### Remove all unused volumes
```sh
docker volume prune
```

---

## Networks

### Create a network
```sh
docker network create <network_name>
```

### List networks
```sh
docker network ls
```

### Connect a container to a network
```sh
docker network connect <network_name> <container_id>
```

### Remove all unused networks
```sh
docker network prune
```

---

## Cleanup

### Remove all unused objects (containers, networks, images, cache)
```sh
docker system prune
```

### Remove all unused objects including volumes
```sh
docker system prune --volumes
```

### Remove all unused images (not just dangling)
```sh
docker image prune -a
```

---

## Docker Compose

### Start services
```sh
docker compose up -d
```

### Stop services
```sh
docker compose down
```

### Stop services and remove volumes
```sh
docker compose down -v
```

### View logs
```sh
docker compose logs -f
```

### Rebuild and start services
```sh
docker compose up -d --build
```

### List running services
```sh
docker compose ps
```
