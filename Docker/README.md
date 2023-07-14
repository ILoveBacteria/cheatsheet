# Docker Cheatsheet

## Table Of Contents
- [Docker Cheatsheet](#docker-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Commands](#commands)
    - [Basics](#basics)
    - [Network](#network)
  - [Dockerfile](#dockerfile)

## Commands

### Basics

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
- `docker build <path>`: Build an image from a Dockerfile

### Network

- `docker network ls`: List networks
- `docker network create <name>`: Create a network
- `docker network rm <name>`: Remove a network
- `docker run --network <name> <image>`: Run a command in a new container and connect it to a network

## Dockerfile

```Dockerfile
FROM python
WORKDIR /usr/src/app
COPY . .
EXPOSE 3000
CMD [ "python", "main.py" ]
```

`CMD`: Tells the docker to what to do when the image runs.
`EXPOSE`: Exposing port 3000 informs Docker which port the container is listening on at runtime.