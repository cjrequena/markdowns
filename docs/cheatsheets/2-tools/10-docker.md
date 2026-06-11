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
- [Dockerfile Reference](https://docs.docker.com/reference/dockerfile/)
- [Docker Compose Reference](https://docs.docker.com/compose/compose-file/)

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

### Build with build arguments
```sh
docker build --build-arg <ARG_NAME>=<value> -t <image_name>:<tag> .
```

### Build with no cache
```sh
docker build --no-cache -t <image_name>:<tag> .
```

### Build for a specific platform
```sh
docker build --platform linux/amd64 -t <image_name>:<tag> .
```

### Multi-platform build (requires buildx)
```sh
docker buildx build --platform linux/amd64,linux/arm64 -t <image_name>:<tag> --push .
```

### List images
```sh
docker images
```

### List images with filters
```sh
docker images --filter "dangling=true"
docker images --filter "reference=<image_name>:*"
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

### Save an image to a tar file
```sh
docker save -o <file_name>.tar <image_name>:<tag>
```

### Load an image from a tar file
```sh
docker load -i <file_name>.tar
```

### View image history (layers)
```sh
docker history <image_name>:<tag>
```

### Inspect an image
```sh
docker inspect <image_name>:<tag>
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

### Run a container with env file
```sh
docker run -d --name <container_name> --env-file .env <image_name>:<tag>
```

### Run a container with a volume mount
```sh
docker run -d --name <container_name> -v <host_path>:<container_path> <image_name>:<tag>
```

### Run a container with a read-only volume
```sh
docker run -d --name <container_name> -v <host_path>:<container_path>:ro <image_name>:<tag>
```

### Run a container with a named volume
```sh
docker run -d --name <container_name> -v <volume_name>:<container_path> <image_name>:<tag>
```

### Run a container with a tmpfs mount
```sh
docker run -d --name <container_name> --tmpfs /tmp <image_name>:<tag>
```

### Run a container with restart policy
```sh
docker run -d --name <container_name> --restart=always <image_name>:<tag>
# Options: no, on-failure, on-failure:5, always, unless-stopped
```

### Run a container with resource limits
```sh
docker run -d --name <container_name> --memory=512m --cpus=1.5 <image_name>:<tag>
```

### Run a container with a specific network
```sh
docker run -d --name <container_name> --network <network_name> <image_name>:<tag>
```

### Run a container with a hostname
```sh
docker run -d --name <container_name> --hostname <hostname> <image_name>:<tag>
```

### Run a container with a specific user
```sh
docker run -d --name <container_name> --user <uid>:<gid> <image_name>:<tag>
```

### Run a container in interactive mode
```sh
docker run -it --name <container_name> <image_name>:<tag> /bin/bash
```

### Run a container and remove it after it stops
```sh
docker run --rm <image_name>:<tag>
```

### Run a container with health check
```sh
docker run -d --name <container_name> \
  --health-cmd="curl -f http://localhost/ || exit 1" \
  --health-interval=30s \
  --health-timeout=10s \
  --health-retries=3 \
  <image_name>:<tag>
```

### Run a container with PID limit
```sh
docker run -d --name <container_name> --pids-limit=100 <image_name>:<tag>
```

### List running containers
```sh
docker ps
```

### List all containers
```sh
docker ps -a
```

### List containers with custom format
```sh
docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}\t{{.Ports}}"
```

### Stop a container
```sh
docker stop <container_id>
```

### Stop a container with timeout
```sh
docker stop -t 30 <container_id>
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

### Pause a container
```sh
docker pause <container_id>
```

### Unpause a container
```sh
docker unpause <container_id>
```

### Remove a container
```sh
docker rm <container_id>
```

### Force remove a running container
```sh
docker rm -f <container_id>
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

### Execute a command in a running container
```sh
docker exec <container_id> <command>
```

### Execute a command as a specific user
```sh
docker exec -u <user> <container_id> <command>
```

### View container logs
```sh
docker logs <container_id>
```

### Follow container logs
```sh
docker logs -f <container_id>
```

### View logs with timestamps
```sh
docker logs -t <container_id>
```

### View last N lines of logs
```sh
docker logs --tail 100 <container_id>
```

### View logs since a specific time
```sh
docker logs --since 2024-01-01T00:00:00 <container_id>
```

### Copy files from container to host
```sh
docker cp <container_id>:<container_path> <host_path>
```

### Copy files from host to container
```sh
docker cp <host_path> <container_id>:<container_path>
```

### View container resource usage
```sh
docker stats <container_id>
```

### View resource usage of all containers
```sh
docker stats
```

### View container processes
```sh
docker top <container_id>
```

### View container port mappings
```sh
docker port <container_id>
```

### Rename a container
```sh
docker rename <old_name> <new_name>
```

### Create an image from a container
```sh
docker commit <container_id> <image_name>:<tag>
```

### Export a container filesystem as a tar archive
```sh
docker export <container_id> > <file_name>.tar
```

### View container changes (diff)
```sh
docker diff <container_id>
```

### Wait for a container to stop and get exit code
```sh
docker wait <container_id>
```

---

## Inspect

### Get IP address of a running container
```sh
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_id>
```

### Get container environment variables
```sh
docker inspect -f '{{range .Config.Env}}{{println .}}{{end}}' <container_id>
```

### Get container mount points
```sh
docker inspect -f '{{range .Mounts}}{{.Source}} -> {{.Destination}}{{println}}{{end}}' <container_id>
```

### Get container restart count
```sh
docker inspect -f '{{.RestartCount}}' <container_id>
```

### Get container health status
```sh
docker inspect -f '{{.State.Health.Status}}' <container_id>
```

### See all space Docker takes up
```sh
docker system df
```

### See detailed space usage
```sh
docker system df -v
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

### Create a volume with a specific driver
```sh
docker volume create --driver <driver_name> <volume_name>
```

### List volumes
```sh
docker volume ls
```

### Inspect a volume
```sh
docker volume inspect <volume_name>
```

### Remove a volume
```sh
docker volume rm <volume_name>
```

### Remove all unused volumes
```sh
docker volume prune
```

### Backup a volume
```sh
docker run --rm -v <volume_name>:/data -v $(pwd):/backup alpine tar czf /backup/<backup_name>.tar.gz -C /data .
```

### Restore a volume
```sh
docker run --rm -v <volume_name>:/data -v $(pwd):/backup alpine tar xzf /backup/<backup_name>.tar.gz -C /data
```

---

## Networks

### Create a network
```sh
docker network create <network_name>
```

### Create a network with a specific subnet
```sh
docker network create --subnet=172.18.0.0/16 <network_name>
```

### Create a network with a specific driver
```sh
docker network create --driver bridge <network_name>
# Drivers: bridge, host, overlay, macvlan, none
```

### List networks
```sh
docker network ls
```

### Inspect a network
```sh
docker network inspect <network_name>
```

### Connect a container to a network
```sh
docker network connect <network_name> <container_id>
```

### Connect a container to a network with a specific IP
```sh
docker network connect --ip 172.18.0.10 <network_name> <container_id>
```

### Disconnect a container from a network
```sh
docker network disconnect <network_name> <container_id>
```

### Remove a network
```sh
docker network rm <network_name>
```

### Remove all unused networks
```sh
docker network prune
```

---

## Registry & Login

### Login to Docker Hub
```sh
docker login
```

### Login to a private registry
```sh
docker login <registry_url>
```

### Logout from a registry
```sh
docker logout <registry_url>
```

### Login to AWS ECR
```sh
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com
```

### Tag and push to a private registry
```sh
docker tag <image_name>:<tag> <registry_url>/<image_name>:<tag>
docker push <registry_url>/<image_name>:<tag>
```

### Search Docker Hub
```sh
docker search <term>
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

### Remove all unused objects (force, no confirmation)
```sh
docker system prune -af --volumes
```

### Remove all unused images (not just dangling)
```sh
docker image prune -a
```

### Remove stopped containers
```sh
docker container prune
```

### Remove build cache
```sh
docker builder prune
```

### Remove all build cache (force)
```sh
docker builder prune -af
```

---

## Dockerfile Instructions

### Common Dockerfile instructions reference

```dockerfile
# Base image
FROM <image_name>:<tag>

# Multi-stage build
FROM <image_name>:<tag> AS builder

# Set metadata
LABEL maintainer="<name>"
LABEL version="1.0"

# Set environment variables
ENV <KEY>=<VALUE>

# Set build-time variables
ARG <ARG_NAME>=<default_value>

# Set working directory
WORKDIR /app

# Copy files from host to image
COPY <src> <dest>

# Copy files with ownership
COPY --chown=<user>:<group> <src> <dest>

# Copy from a build stage
COPY --from=builder /app/build /app

# Add files (supports URLs and auto-extraction of tar files)
ADD <src> <dest>

# Run a command during build
RUN <command>

# Run multiple commands in a single layer
RUN apt-get update && \
    apt-get install -y <package> && \
    rm -rf /var/lib/apt/lists/*

# Set the default command
CMD ["executable", "param1", "param2"]

# Set the entrypoint
ENTRYPOINT ["executable"]

# Expose a port
EXPOSE <port>

# Define a volume
VOLUME ["/data"]

# Set the user
USER <user>:<group>

# Health check
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost/ || exit 1

# Disable health check
HEALTHCHECK NONE

# Signal to stop the container
STOPSIGNAL SIGTERM

# Set shell
SHELL ["/bin/bash", "-c"]
```

### Multi-stage build example

```dockerfile
# Build stage
FROM golang:1.21-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o main .

# Production stage
FROM alpine:3.19
RUN apk --no-cache add ca-certificates
WORKDIR /app
COPY --from=builder /app/main .
EXPOSE 8080
USER nobody:nobody
ENTRYPOINT ["./main"]
```

### Java Spring Boot example

```dockerfile
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY target/*.jar app.jar
EXPOSE 8080
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost:8080/actuator/health || exit 1
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### Node.js example

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
USER node
CMD ["node", "index.js"]
```

---

## Docker Compose

### Start services
```sh
docker compose up -d
```

### Start a specific service
```sh
docker compose up -d <service_name>
```

### Stop services
```sh
docker compose down
```

### Stop services and remove volumes
```sh
docker compose down -v
```

### Stop services and remove images
```sh
docker compose down --rmi all
```

### View logs
```sh
docker compose logs -f
```

### View logs for a specific service
```sh
docker compose logs -f <service_name>
```

### Rebuild and start services
```sh
docker compose up -d --build
```

### Rebuild a specific service
```sh
docker compose up -d --build <service_name>
```

### Force recreate containers
```sh
docker compose up -d --force-recreate
```

### Scale a service
```sh
docker compose up -d --scale <service_name>=3
```

### List running services
```sh
docker compose ps
```

### List all services (including stopped)
```sh
docker compose ps -a
```

### Execute a command in a service
```sh
docker compose exec <service_name> <command>
```

### Run a one-off command in a service
```sh
docker compose run --rm <service_name> <command>
```

### Pull latest images
```sh
docker compose pull
```

### View service configuration
```sh
docker compose config
```

### Validate compose file
```sh
docker compose config --quiet
```

### View service resource usage
```sh
docker compose top
```

### Restart a specific service
```sh
docker compose restart <service_name>
```

### Pause/unpause services
```sh
docker compose pause
docker compose unpause
```

### Docker Compose file example

```yaml
version: "3.9"

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
      args:
        - APP_ENV=production
    image: <image_name>:<tag>
    container_name: <container_name>
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=postgresql://<user>:<password>@db:5432/<db_name>
    env_file:
      - .env
    volumes:
      - app-data:/app/data
    networks:
      - app-network
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped
    deploy:
      resources:
        limits:
          cpus: "1.0"
          memory: 512M
        reservations:
          cpus: "0.5"
          memory: 256M
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

  db:
    image: postgres:16-alpine
    container_name: db
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_USER=<user>
      - POSTGRES_PASSWORD=<password>
      - POSTGRES_DB=<db_name>
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - app-network
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U <user>"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  app-data:
  db-data:

networks:
  app-network:
    driver: bridge
```

---

## Docker Context (Remote Management)

### List contexts
```sh
docker context ls
```

### Create a context for a remote host
```sh
docker context create <context_name> --docker "host=ssh://<user>@<host>"
```

### Switch context
```sh
docker context use <context_name>
```

### Remove a context
```sh
docker context rm <context_name>
```

---

## Security Best Practices

### Run a container as non-root
```sh
docker run -d --user 1000:1000 <image_name>:<tag>
```

### Run a container with read-only filesystem
```sh
docker run -d --read-only <image_name>:<tag>
```

### Run a container with no new privileges
```sh
docker run -d --security-opt=no-new-privileges <image_name>:<tag>
```

### Drop all capabilities and add only needed ones
```sh
docker run -d --cap-drop ALL --cap-add NET_BIND_SERVICE <image_name>:<tag>
```

### Scan an image for vulnerabilities
```sh
docker scout cves <image_name>:<tag>
```

### View image SBOM (Software Bill of Materials)
```sh
docker sbom <image_name>:<tag>
```

---

## Useful Tips

### View Docker version
```sh
docker version
```

### View Docker system info
```sh
docker info
```

### View Docker events in real-time
```sh
docker events
```

### Format output as JSON
```sh
docker ps --format '{{json .}}'
```

### Update container restart policy
```sh
docker update --restart=always <container_id>
```

### Update container resource limits
```sh
docker update --memory=1g --cpus=2 <container_id>
```
