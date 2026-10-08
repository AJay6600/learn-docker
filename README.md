# User App (React + Node + MongoDB)

## About This Project

This project is built for **learning Docker**. It is a simple full-stack user app (React frontend, Node.js backend, MongoDB database) used to practice:

- Dockerizing an app (writing a Dockerfile and building images)
- Running and managing containers
- Port binding, networks and volumes
- Running multiple services together with Docker Compose

Project parts:

- `frontend/` : React (Vite) app. See `frontend/README.md`
- `backend/` : Node.js API. See `backend/README.md`
- `docker.yaml` : Docker Compose file that runs everything

This file lists the Docker commands used while learning.

---

## Run the Project Locally (Without Docker for the Apps)

Follow the steps in each app's README, in this order:

1. Start MongoDB and run the backend: [backend/README.md](./backend/README.md)
2. Run the frontend: [frontend/README.md](./frontend/README.md)

Then open `http://localhost:5173`.

---

## Run the Project with Docker (After Cloning)

Use these steps if you cloned the project and want to start everything in Docker containers.

### Requirements

- Git
- Docker Desktop (running)

Check that Docker is installed:

```bash
docker --version
docker compose version
```

### Steps

1. Clone the project:

```bash
git clone <repository-url>
```

2. Go into the project folder (the one containing `docker.yaml`):

```bash
cd <project-folder>
```

3. Build and start all containers (MongoDB, Mongo Express, backend, frontend):

```bash
docker compose -f docker.yaml up -d --build
```

4. Check that all containers are running:

```bash
docker compose -f docker.yaml ps
```

5. Open the app in your browser:

| Service       | URL                   |
|---------------|-----------------------|
| Frontend      | http://localhost:5173 |
| Backend API   | http://localhost:5000 |
| Mongo Express | http://localhost:8081 |

6. If something does not work, check the logs:

```bash
docker compose -f docker.yaml logs -f backend
```

7. Stop the project (data is kept):

```bash
docker compose -f docker.yaml down
```

8. Stop and delete all data (fresh start):

```bash
docker compose -f docker.yaml down -v
```

Make sure ports `5173`, `5000`, `8081` and `27017` are free before starting.

---

## 1. General Docker Commands

```bash
docker --version
docker info
docker help
docker <command> --help
docker login
docker logout
docker system df
docker system prune
docker system prune -a
docker system prune -a --volumes
```

## 2. Docker Image Commands

```bash
docker images
docker image ls
docker pull mongo
docker pull node:20-alpine
docker search mongo
docker rmi <image_name_or_id>
docker rmi -f <image_id>
docker image prune
docker image prune -a
docker image inspect <image>
docker image history <image>
docker tag backend-app:latest myuser/backend-app:v1
docker push myuser/backend-app:v1
```

## 3. Build Commands (Dockerization)

```bash
docker build -t backend-app:latest ./backend
docker build -t frontend:v1 ./frontend
docker build -t frontend:v1 .
docker build -t frontend:v1 -f path/to/Dockerfile .
docker build --no-cache -t frontend:v1 .
```

## 4. Container Commands

```bash
docker ps
docker ps -a
docker ps -q
docker inspect <container>
docker logs <container>
docker logs -f <container>
docker logs --tail 50 <container>
docker stats
docker top <container>
docker rename old_name new_name
docker cp file.txt <container>:/app/
docker cp <container>:/app/file.txt .
docker rm <container>
docker rm -f <container>
docker container prune
```

## 5. Run, Start, Stop Commands

```bash
docker run <image>
docker run -d <image>
docker run -d --name myapp <image>
docker run -d -p 5000:5000 <image>
docker run -d -e KEY=value <image>
docker run -d -v mongo-data:/data/db <image>
docker run -d --network mongo-network <image>
docker run --rm <image>
docker run -it <image> sh

docker start <container>
docker stop <container>
docker restart <container>
docker pause <container>
docker unpause <container>
docker kill <container>
```

Note for zsh: wrap values containing `?` in quotes, for example `-e "MONGO_URI=...?authSource=admin"`.

## 6. Bash Commands (Go Inside a Container)

```bash
docker exec -it <container> bash
docker exec -it <container> sh
docker exec <container> ls /app
docker exec -it mongodb mongosh -u admin -p <password> --authenticationDatabase admin
exit
```

## 7. Port Binding

Format: `-p HOST_PORT:CONTAINER_PORT`

```bash
docker run -d -p 5000:5000 backend-app
docker run -d -p 8080:5000 backend-app
docker run -d -p 127.0.0.1:5000:5000 backend-app
docker run -d -P backend-app
docker port <container>
```

## 8. Docker Network

```bash
docker network ls
docker network create mongo-network
docker network inspect mongo-network
docker network connect mongo-network <container>
docker network disconnect mongo-network <container>
docker network rm mongo-network
docker network prune
```

## 9. Docker Volumes

```bash
docker volume create mongo-data
docker volume ls
docker volume inspect mongo-data
docker volume rm mongo-data
docker volume prune
docker run -d --name mongodb -v mongo-data:/data/db mongo
docker run --rm -v mongo-data:/data alpine ls /data
```

## 10. Docker Compose

```bash
docker compose -f docker.yaml up
docker compose -f docker.yaml up -d
docker compose -f docker.yaml up -d --build
docker compose -f docker.yaml down
docker compose -f docker.yaml down -v
docker compose -f docker.yaml ps
docker compose -f docker.yaml logs
docker compose -f docker.yaml logs -f backend
docker compose -f docker.yaml restart backend
docker compose -f docker.yaml stop
docker compose -f docker.yaml start
docker compose -f docker.yaml build
docker compose -f docker.yaml exec mongo bash
docker compose -f docker.yaml config
```

## 11. Troubleshooting Commands

```bash
docker ps -a
docker logs -f <container>
docker inspect <container>
docker exec -it <container> sh
curl http://localhost:5000/health
docker rm -f <container>
```

## 12. Quick Cheat Sheet

```bash
docker build -t backend-app ./backend
docker run -d --name backendapp -p 5000:5000 backend-app
docker ps
docker logs -f backendapp
docker exec -it backendapp sh
docker stop backendapp
docker rm backendapp
docker rmi backend-app

docker compose -f docker.yaml up -d --build
docker compose -f docker.yaml down
```