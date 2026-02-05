# Docker Commands Cheat Sheet (2025–2026)

Comprehensive list of the most useful **Docker CLI** commands, organized by topic/category, with description and practical examples.

> **Note**: Most commands accept `--help` to see all flags  
> Most management commands support short format: `docker ps` = `docker container ls`  
> Use `docker <command> --help` for detailed options

## 1. General & Info Commands

| Command                          | Description                                      | Example
|----------------------------------|--------------------------------------------------|---------------------------------------------------------------
| `docker --version`               | Show Docker version                              | `docker --version`
| `docker version`                 | Detailed version info (client + server)          | `docker version --format '{{.Server.Version}}'`
| `docker info`                    | Show system-wide information                     | `docker info --format '{{.MemTotal}}'`
| `docker system df`               | Show disk usage (images, containers, volumes)    | `docker system df -v`
| `docker system prune`            | Remove unused data (containers, images, networks, cache) | `docker system prune -a --volumes`

## 2. Image Management

| Command                              | Description                                      | Example
|--------------------------------------|--------------------------------------------------|-----------------------------------------------------------------
| `docker images` / `docker image ls`  | List local images                                | `docker images -a`
| `docker pull <image>`                | Download image from registry                     | `docker pull nginx:1.25-alpine`
| `docker push <image>`                | Upload image to registry                         | `docker push myuser/webapp:1.0`
| `docker build -t <name:tag> .`       | Build image from Dockerfile                      | `docker build -t myapp:dev .`
| `docker build --no-cache ...`        | Build without cache                              | `docker build --no-cache -t myapp:clean .`
| `docker rmi <image>`                 | Remove image                                     | `docker rmi nginx:1.25-alpine`
| `docker image prune`                 | Remove dangling (untagged) images                | `docker image prune -a`
| `docker search <term>`               | Search Docker Hub                                | `docker search --filter stars=500 redis`
| `docker tag <source> <target>`       | Create new tag for image                         | `docker tag myapp:dev myapp:latest`

## 3. Container Management

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------
| `docker run ...`                             | Create & start container                             | `docker run -d -p 8080:80 --name web nginx`
| `docker ps` / `docker container ls`          | List running containers                              | `docker ps -a` (all containers)
| `docker start <container>`                   | Start stopped container                              | `docker start web`
| `docker stop <container>`                    | Gracefully stop container                            | `docker stop web`
| `docker restart <container>`                 | Restart container                                    | `docker restart web`
| `docker rm <container>`                      | Remove stopped container                             | `docker rm web`
| `docker rm -f <container>`                   | Force remove (even running)                          | `docker rm -f web`
| `docker container prune`                     | Remove all stopped containers                        | `docker container prune`
| `docker rename <old> <new>`                  | Rename container                                     | `docker rename old-web prod-web`

## 4. Interacting with Running Containers

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------
| `docker exec -it <container> bash`           | Open interactive shell                               | `docker exec -it web bash`
| `docker exec <container> <cmd>`              | Run command non-interactively                        | `docker exec web ls /usr/share/nginx/html`
| `docker logs <container>`                    | Show container logs                                  | `docker logs -f --tail 100 web`
| `docker attach <container>`                  | Attach to container stdin/stdout                     | `docker attach web`
| `docker cp <container>:/path ./local`        | Copy file/dir from container to host                 | `docker cp web:/etc/nginx/nginx.conf ./`
| `docker cp ./local <container>:/path`        | Copy file/dir from host to container                 | `docker cp index.html web:/usr/share/nginx/html/`

## 5. Resource & Stats

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------
| `docker stats`                               | Live resource usage (CPU, memory, etc.)              | `docker stats --no-stream`
| `docker top <container>`                     | Show running processes in container                  | `docker top web`
| `docker inspect <container/image>`           | Detailed info (JSON)                                 | `docker inspect --format '{{.State.Pid}}' web`

## 6. Volumes

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------
| `docker volume ls`                           | List volumes                                         | `docker volume ls`
| `docker volume create <name>`                | Create named volume                                  | `docker volume create mydata`
| `docker volume inspect <name>`               | Show volume details                                  | `docker volume inspect mydata`
| `docker volume rm <name>`                    | Remove volume                                        | `docker volume rm mydata`
| `docker volume prune`                        | Remove all unused volumes                            | `docker volume prune`
| `docker run -v /host/path:/container/path`   | Bind mount                                           | `docker run -v $(pwd)/data:/app/data ...`
| `docker run -v myvol:/container/path`        | Named volume                                         | `docker run -v mydata:/var/lib/mysql ...`

## 7. Networks

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------
| `docker network ls`                          | List networks                                        | `docker network ls`
| `docker network create <name>`               | Create custom network                                | `docker network create mynet`
| `docker network inspect <name>`              | Show network details                                 | `docker network inspect bridge`
| `docker network rm <name>`                   | Remove network                                       | `docker network rm mynet`
| `docker network prune`                       | Remove unused networks                               | `docker network prune`
| `docker run --network <name>`                | Connect container to network                         | `docker run --network mynet ...`

## 8. Docker Compose (very commonly used)

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------
| `docker compose up`                          | Create & start services                              | `docker compose up -d`
| `docker compose down`                        | Stop & remove containers, networks, volumes*         | `docker compose down --volumes`
| `docker compose ps`                          | List containers                                      | `docker compose ps`
| `docker compose logs`                        | Show logs                                            | `docker compose logs -f api`
| `docker compose build`                       | Build or rebuild services                            | `docker compose build --no-cache`
| `docker compose exec <service> <cmd>`        | Execute command in running service                   | `docker compose exec web bash`

## 9. Cleanup & Maintenance (most used in production)

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------
| `docker system prune -a --volumes`           | Nuclear cleanup (everything unused)                  | `docker system prune -a --volumes --force`
| `docker image prune -a`                      | Remove all unused images                             | `docker image prune -a`
| `docker builder prune`                       | Clear build cache                                    | `docker builder prune --all`

## Quick One-liners (Daily DevOps)

```bash
# Rebuild & restart single service
docker compose up -d --build api

# Enter running container quickly
docker exec -it $(docker ps -qf "name=web") bash

# Remove all stopped containers + dangling images
docker container prune && docker image prune

# Watch logs of latest container
docker logs -f $(docker ps -lq)
