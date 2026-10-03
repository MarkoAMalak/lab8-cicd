# Lab 8: CI/CD for a Multi-Container App

A Node.js (Express) API backed by MongoDB, packaged with Docker Compose and deployed to an
AWS EC2 instance by GitHub Actions.

## Architecture

```
git push (main) ──► GitHub Actions
                     ├─ build-and-push: build image ──► Docker Hub (<user>/lab6-app:latest)
                     └─ deploy: SSH to EC2 ──► git pull ──► docker-compose pull ──► up -d
EC2: app (Express, port 3000) ──► db (MongoDB, seeded by init.js)
```

## Endpoints

| Route | Returns |
|---|---|
| `GET /` | service info (host name, database status) |
| `GET /tasks` | the tasks stored in MongoDB |

## Run locally

```bash
docker-compose up --build
curl http://localhost:3000/
curl http://localhost:3000/tasks
```

## CI/CD setup

The workflow in `.github/workflows/deploy.yml` needs these repository secrets:

| Secret | Purpose |
|---|---|
| `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN` | push the image to Docker Hub |
| `EC2_HOST`, `EC2_SSH_KEY` | SSH into the EC2 instance |

On the EC2 instance, clone this repository to `~/lab8-cicd`. The deploy job writes a `.env`
file with `DOCKERHUB_USERNAME`, so `docker-compose pull` fetches the image CI just pushed.

The assignment brief is in `Lab 8 CI_CD_Assignment.pdf`.

## Author

Marko A. Malak, CISC 886, Queen's University
