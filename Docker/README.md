# Docker Cheatsheet

## Table Of Contents
- [Docker Cheatsheet](#docker-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Commands](#commands)
    - [Basics](#basics)
    - [Push](#push)
    - [Options](#options)
    - [Network](#network)
  - [Dockerfile](#dockerfile)
    - [Sample Dockerfile for Python](#sample-dockerfile-for-python)
  - [Docker Compose](#docker-compose)

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
- `docker image prune -a`: Remove all images.

### Push

1. `docker tag my-image username/my-repo`
2. `docker push username/my-repo`

### Options

- `--name`: Assign a name to container.
- `--network`: `none` - `bridge` - `host` - `network-name|network-id`
- `--restart`: `on-failure[:max-retries]` - `always` - `no` - `unless-stopped`
- `--rm`: Automatically remove the container when it exits.

### Network

- `docker network ls`: List networks
- `docker network create <name>`: Create a network
- `docker network rm <name>`: Remove a network
- `docker run --network <name> <image>`: Run a command in a new container and connect it to a network

## Dockerfile

- `docker build -t <my-app>:<tag> <path>`: Build an image from a Dockerfile

```Dockerfile
FROM python:3.10
WORKDIR /usr/src/app
COPY . .
RUN pip install pipenv
RUN pipenv install
EXPOSE 10002
ENTRYPOINT [ "pipenv", "run", "python", "./main.py" ]
CMD [ "0.0.0.0" ]
```

- `CMD`: Tells the docker to what to do when the image runs (by default parameters).
- `RUN`: Run a command like in a terminal.
- `EXPOSE`: Exposing port 3000 informs Docker which port the container is listening on at runtime.

if we run this `docker run <image>` the command `pipenv run python main.py 0.0.0.0` will be run. But if we run this `docker run <image> 127.0.0.1` the command `pipenv run python main.py 127.0.0.1` will be run

### Sample Dockerfile for Python

```Dockerfile
FROM python:3.10-slim-bullseye

WORKDIR /usr/src/app

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

COPY ./requirements.txt .
RUN pip install -r requirements.txt
COPY . .

EXPOSE 8000

ENTRYPOINT [ "/usr/src/app/docker-entrypoint.sh" ]
```

Why `PYTHONDONTWRITEBYTECODE`? To reduce the image size.

Why `PYTHONUNBUFFERED`? To not buffer the stdout and stderr.

```bash
#!/bin/sh

python manage.py migrate
python manage.py collectstatics --no-input
python manage.py runserver
```

## Docker Compose

```yml
services:
  web:
    container_name: custom_name
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
  redis:
    image: redis
```

```shell
docker-compose logs

docker-compose pause  # pause the environment execution without changing the current state of your containers
docker-compose unpause

docker-compose stop # terminate the container execution, but it won’t destroy any data

docker-compose down # remove the containers, networks, and volumes
```

docker compose file sample:

```yml
version: "3.9"

services:
  web:
    container_name: toolbox
    build: .
    ports:
      - "8000:8000"
    env_file:
      - ./.env
    depends_on:
      redis:
        condition: service_healthy
    healthcheck:
      test: curl --fail http://127.0.0.1:8000/ || exit 1
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 5s
  redis:
    container_name: redis
    image: redis:7.2
    healthcheck:
      test: redis-cli ping || exit 1
      start_period: 5s
      interval: 10s
      timeout: 10s
      retries: 3
```