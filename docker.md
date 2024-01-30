# Docker

## Docker container

- Isolate environment => no knowledge of your operation system or your file
- Runs on the environment provided to you by `Docker Desktop`
- Containers have everything that your code needs in order to run, down to a base operating system
- `docker init`
- `docker build -t welcome-to-docker .` to build image from a Dockerfile
- `docker run -dp 127.0.0.1:3000:3000 getting-started` to start an app Container
- `docker ps` to list containers

## Multi-container app

- Docker Compose could start multiple containers with a single command
- `docker compose up -d` to run multiple containers
- `docker compose watch` to see real time changes

## Persist container data

- `Volume` helps data persistence
- Set it in `compose.yaml` file

## Access a local folder

- `Bind mount`
- Set it in `compose.yaml` file
