# Docker Commands Cheat Sheet – Extended & More Complete (2026)

Organized by topic.  
Most commands have short + long form: `docker ps` = `docker container ls`  
Use `--help` on any command for full flags/options.  
Run `docker <command> --help` or `docker --help` for quick overview.

## 1. General & System Information

| Command                              | Description                                      | Example / Common Flags
|--------------------------------------|--------------------------------------------------|-----------------------------------------------
| `docker --version`                   | Docker CLI + Engine version                      | `docker --version`
| `docker version`                     | Detailed client/server versions                  | `docker version --format '{{json .}}'`
| `docker info`                        | System-wide info (containers, images, storage)   | `docker info --format '{{.MemTotal}}'`
| `docker system df`                   | Disk usage summary (images/containers/volumes/cache) | `docker system df -v`
| `docker system events`               | Live events stream (create/start/stop/pull/…)    | `docker system events --since '30m'`
| `docker system prune`                | Remove unused objects                            | `docker system prune -a --volumes --force`
| `docker system dial-stdio`           | Low-level debug (rare)                           | —

## 2. Image Management

| Command                              | Description                                      | Example
|--------------------------------------|--------------------------------------------------|-----------------------------------------------
| `docker images` / `docker image ls`  | List images                                      | `docker images --digests --filter "dangling=true"`
| `docker pull <image>`                | Pull from registry                               | `docker pull redis:7-alpine`
| `docker push <image>`                | Push to registry                                 | `docker push myrepo/webapp:v2`
| `docker build -t <name:tag> .`       | Build from Dockerfile (legacy builder)           | `docker build -t myapp .`
| `docker buildx build …`              | Modern Buildx builder (recommended)              | `docker buildx build --push -t myapp .`
| `docker rmi <image>`                 | Remove image(s)                                  | `docker rmi -f nginx`
| `docker image prune`                 | Remove unused/dangling images                    | `docker image prune -a`
| `docker tag <source> <target>`       | Retag existing image                             | `docker tag alpine:latest my-alpine:prod`
| `docker save -o file.tar <image>`    | Save image(s) to tar file                        | `docker save -o nginx.tar nginx`
| `docker load -i file.tar`            | Load image from tar file                         | `docker load -i backup.tar`
| `docker history <image>`             | Show image layers                                | `docker history --no-trunc myapp`
| `docker search <term>`               | Search Docker Hub (limited now)                  | `docker search --filter is-official=true alpine`

## 3. Container Lifecycle

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|---------------------------------------------------------------
| `docker run [OPTIONS] IMAGE [CMD]`           | Create + start container                             | `docker run -d -p 80:80 --name web -v data:/app nginx`
| `docker create …`                            | Create container (not started)                       | `docker create --name temp nginx`
| `docker start <container>`                   | Start stopped container                              | `docker start web`
| `docker stop <container>`                    | Graceful stop (SIGTERM)                              | `docker stop -t 30 web`
| `docker kill <container>`                    | Force kill (SIGKILL)                                 | `docker kill web`
| `docker restart <container>`                 | Restart container                                    | `docker restart web`
| `docker pause / unpause <container>`         | Pause/unpause processes                              | `docker pause web`
| `docker rm <container>`                      | Remove stopped container                             | `docker rm -f web` (force)
| `docker rename <old> <new>`                  | Rename container                                     | `docker rename old-api api-v2`

## 4. Container Interaction & Debugging

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|---------------------------------------------------------------
| `docker ps / container ls`                   | List running containers                              | `docker ps -a --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"`
| `docker logs <container>`                    | Show logs                                            | `docker logs -f --tail 200 api`
| `docker exec -it <container> sh/bash`        | Run interactive command/shell                        | `docker exec -it web bash`
| `docker attach <container>`                  | Attach to container stdin/stdout                     | `docker attach --detach-keys=ctrl-@ web`
| `docker cp <src> <dest>`                     | Copy files between host ↔ container                  | `docker cp web:/etc/nginx/nginx.conf ./`
| `docker top <container>`                     | Show processes inside container                      | `docker top web`
| `docker stats`                               | Live CPU/memory/network stats                        | `docker stats --no-stream`
| `docker inspect <object>`                    | Detailed JSON metadata                               | `docker inspect --format '{{.State.ExitCode}}' web`
| `docker port <container>`                    | Show published ports                                 | `docker port web`
| `docker wait <container>`                    | Block until container stops                          | `docker wait web`

## 5. Volumes

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|---------------------------------------------------------------
| `docker volume ls`                           | List volumes                                         | `docker volume ls -f dangling=true`
| `docker volume create <name>`                | Create named volume                                  | `docker volume create --driver local pgdata`
| `docker volume inspect <name>`               | Volume details (mountpoint)                          | `docker volume inspect pgdata`
| `docker volume rm <name>`                    | Remove volume                                        | `docker volume rm pgdata`
| `docker volume prune`                        | Remove unused volumes                                | `docker volume prune -f`

## 6. Networks

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|---------------------------------------------------------------
| `docker network ls`                          | List networks                                        | `docker network ls`
| `docker network create <name>`               | Create network                                       | `docker network create --driver bridge mynet`
| `docker network inspect <name>`              | Network details                                      | `docker network inspect bridge`
| `docker network rm <name>`                   | Remove network                                       | `docker network rm mynet`
| `docker network prune`                       | Remove unused networks                               | `docker network prune`

## 7. Docker Compose (v2 plugin – `docker compose`)

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|---------------------------------------------------------------
| `docker compose up`                          | Build + start services                               | `docker compose up -d --build`
| `docker compose down`                        | Stop + remove resources                              | `docker compose down --volumes --remove-orphans`
| `docker compose ps`                          | List containers                                      | `docker compose ps -a`
| `docker compose logs`                        | Service logs                                         | `docker compose logs -f --tail=100 backend`
| `docker compose exec <service> <cmd>`        | Run command in service                               | `docker compose exec backend bash`
| `docker compose build`                       | Build services                                       | `docker compose build --no-cache`
| `docker compose config`                      | Validate + view merged config                        | `docker compose config`
| `docker compose restart`                     | Restart services                                     | `docker compose restart api`

## 8. Buildx (Modern Build – multi-platform, cache, Bake)

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|---------------------------------------------------------------
| `docker buildx ls`                           | List builders                                        | `docker buildx ls`
| `docker buildx create --use`                 | Create/use new builder                               | `docker buildx create --name mybuilder --use`
| `docker buildx build`                        | Advanced build (platforms, push, cache)              | `docker buildx build --platform linux/amd64,linux/arm64 --push -t myapp .`
| `docker buildx bake`                         | Build from compose-like HCL file                     | `docker buildx bake -f docker-bake.hcl`
| `docker buildx prune`                        | Clear build cache                                    | `docker buildx prune --all --filter until=24h`

## 9. Registry & Authentication

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|---------------------------------------------------------------
| `docker login`                               | Log in to registry                                   | `docker login` or `docker login ghcr.io`
| `docker logout`                              | Log out                                              | `docker logout`
| `docker trust`                               | Manage image trust & signing (Notary)                | `docker trust inspect myrepo/app`

## 10. Swarm Mode (orchestration – basics)

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|---------------------------------------------------------------
| `docker swarm init`                          | Initialize Swarm                                     | `docker swarm init --advertise-addr 192.168.1.10`
| `docker swarm join`                          | Join node to swarm                                   | `docker swarm join --token …`
| `docker node ls`                             | List swarm nodes                                     | `docker node ls`
| `docker service create`                      | Create service                                       | `docker service create --name web --replicas 3 -p 80:80 nginx`
| `docker service ls / ps / logs / scale`      | Manage services                                      | `docker service scale web=5`

## 11. Other Useful / Advanced

| Command                                      | Description                                          | Example
|----------------------------------------------|------------------------------------------------------|---------------------------------------------------------------
| `docker import <tar> <name>`                 | Create image from tarball (filesystem snapshot)      | `docker import rootfs.tar myimage:imported`
| `docker export <container>`                  | Export container filesystem to tar                   | `docker export web > web.tar`
| `docker container prune`                     | Remove all stopped containers                        | `docker container prune -f`
| `docker context ls / use`                    | Manage Docker contexts (local/remote/…)              | `docker context use remote-prod`
| `docker plugin ls / install`                 | Manage plugins                                       | `docker plugin ls`
| `docker secret create / ls`                  | Swarm secrets                                        | `echo "mypw" | docker secret create db_pass -`
| `docker config create / ls`                  | Swarm configs                                        | `docker config create app.conf config.json`


## Quick Explanation of the Most Common `docker run` Flags

These three flags appear in almost every real-world `docker run` command:

| Flag       | Full name          | What it does                                                                 | Why we use it                                                                 | Example from your commands
|------------|--------------------|------------------------------------------------------------------------------|-------------------------------------------------------------------------------|------------------------------------
| `-d`       | `--detach`         | Runs the container **in the background** (detached mode) instead of blocking your terminal. Without `-d`, the terminal stays attached — you see logs live, but can't use the terminal for other commands until you press Ctrl+C (which stops the container). | Most services (web servers, databases, APIs) should run silently in the background. You can see logs later with `docker logs`. | All three examples use `-d` so the container keeps running after you press Enter.
| `-p`       | `--publish`        | **Publishes** (maps/exposes) a port from the **container** to the **host** machine. Format: `-p host_port:container_port` (or more advanced variations). Without `-p`, the service inside the container is only reachable by other containers — not from your browser/computer. | Lets you access the service from outside Docker (e.g. open http://localhost:8080 in browser). | `-p 8080:80` → host port 8080 → container port 80<br>`-p 5432:5432` → standard PostgreSQL port<br>`-p 127.0.0.1:4000:4000` → only accessible from localhost (more secure for dev)
| `--name`   | `--name`           | Gives the container a **human-readable name** instead of a random ID (like `happy_turing`). Makes it much easier to work with later (`docker stop web`, `docker logs web`, etc.). | Avoids typing long random container IDs every time. Recommended for important containers. | `--name web` → you can now use `docker stop web` instead of remembering the ID

### Quick Summary Table – What Happens Without These Flags?

| Without flag       | What happens                                                                 | Problem it causes
|--------------------|------------------------------------------------------------------------------|--------------------------
| No `-d`            | Container runs in **foreground** → terminal blocked, logs shown live         | Can't use terminal, Ctrl+C stops container
| No `-p`            | Port **not published** to host → service invisible from your computer        | Can't open http://localhost:xxxx
| No `--name`        | Random name generated (e.g. `festive_fermat`)                                | Hard to remember/reference container

## 1. General & System Information

| Command                        | Description                                      | Example
|--------------------------------|--------------------------------------------------|-----------------------------------------------
| `docker --version`             | Show Docker version                              | `docker --version`
| `docker info`                  | System-wide information                          | `docker info`
| `docker system prune`          | Clean up unused stuff                            | `docker system prune -a --volumes --force`

## 2. Image Management (Quick)

| Command          | Description                   | Example
|------------------|-------------------------------|--------------------------------
| `docker pull`    | Download image                | `docker pull nginx:alpine`
| `docker images`  | List images                   | `docker images`

## 3. Running Containers – Common Patterns

```bash
# 1. Simple web server (background + port mapping + named)
docker run -d -p 8080:80 --name web nginx:alpine

# 2. PostgreSQL database (standard port + env var + background)
docker run -d -p 5432:5432 -e POSTGRES_PASSWORD=secret --name mydb postgres:16

# 3. Your own API – only accessible from your machine (secure)
docker run -d -p 127.0.0.1:4000:4000 --name myapi my-api-image

## Quick Cleanup One-liners (very useful)

```bash
# Aggressive cleanup (careful!)
docker system prune -a --volumes --force

# Stop & remove everything
docker stop $(docker ps -q) && docker rm $(docker ps -aq)

# Remove dangling + unused
docker image prune -a && docker builder prune --all
