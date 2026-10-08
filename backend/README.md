# Backend (Node.js + MongoDB)

API runs on `http://localhost:5000`

## Run Without Docker

```bash
cd backend
npm install
npm run dev
```

MongoDB must be running (see below).

## Start MongoDB (Docker)

```bash
docker network create mongo-network
docker run -d --name mongodb --network mongo-network -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password123 mongo
```

## Build the Docker Image

```bash
cd backend
docker build -t backend-app:latest .
docker images
```

## Run the Docker Container

```bash
docker run -d \
  --name backendapp \
  -p 5000:5000 \
  --network mongo-network \
  -e PORT=5000 \
  -e "MONGO_URI=mongodb://username:password@mongodb:27017/testdb?authSource=admin" \
  backend-app:latest
```

## Useful Commands

```bash
docker ps
docker logs -f backendapp
docker exec -it backendapp sh
docker stop backendapp
docker start backendapp
docker rm -f backendapp
docker rmi backend-app:latest
curl http://localhost:5000/health
```

## API

- POST /api/users
- GET /api/users
- GET /health