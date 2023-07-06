# Docker Cheatsheet

## Table Of Contents
- [Docker Cheatsheet](#docker-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Commands](#commands)

## Commands

- `docker pull <image>`: Pull an image or a repository from a registry
- `docker images`: List images
- `docker run <image>`: Run a command in a new container
- `docker run -it <image> <command>`: Run a command in a new container with interactive mode
- `docker run --rm <image>`: Run a command in a new container and remove it after it exits
- `docker ps`: List running containers
- `docker ps -a`: List archived containers
- `docker rm <container>`: Remove one or more containers
- `docker container prune`: Remove all stopped containers
- `docker rmi <image>`: Remove one or more images
- `docker exec <container> <command>`: Run a command in a running container
- `docker run -p5000:5000 <image>`: Run a command in a new container and map port 5000 to 5000