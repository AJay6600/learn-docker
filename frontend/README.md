# Frontend (React + Vite)

App runs on `http://localhost:5173`

## Run Without Docker

```bash
cd frontend
npm install
npm run dev
```

## Build the Docker Image

```bash
cd frontend
docker build -t frontend:v1 .
docker images
```

## Run the Docker Container

```bash
docker run -d --name frontendapp -p 5173:5173 frontend:v1
```

## Useful Commands

```bash
docker ps
docker logs -f frontendapp
docker exec -it frontendapp sh
docker stop frontendapp
docker start frontendapp
docker rm -f frontendapp
docker rmi frontend:v1
```