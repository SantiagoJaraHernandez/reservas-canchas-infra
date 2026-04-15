# Reservas Canchas Infra

Infrastructure repository for local multi-environment backend deployment.

## Description

This repository centralizes environment orchestration for the backend services:

- development
- quality assurance
- main

It includes Docker Compose files for each environment and allows the backend to run with isolated ports, networks, volumes, and container names.

## Managed Services

- gateway
- ms-auth
- ms-reservas
- PostgreSQL
- MongoDB

## Repository Layout

```text
docker-compose.dev.yml
docker-compose.qa.yml
docker-compose.main.yml
postman/
README.md
````

## Required Local Folder Structure

All repositories must be cloned as sibling folders:

```text
D:\Proyectos\Sistemas Distribuidos\
├── reservas-canchas-gateway
├── reservas-canchas-ms-auth
├── reservas-canchas-ms-reservas
└── reservas-canchas-infra
```

## Build Requirement

Before running Docker Compose, each microservice jar must be generated.

### Gateway

```powershell
cd ..\reservas-canchas-gateway
.\mvnw.cmd clean package -DskipTests
```

### MS Auth

```powershell
cd ..\reservas-canchas-ms-auth
.\mvnw.cmd clean package -DskipTests
```

### MS Reservas

```powershell
cd ..\reservas-canchas-ms-reservas
.\mvnw.cmd clean package -DskipTests
```

Then return to this repository:

```powershell
cd ..\reservas-canchas-infra
```

## Start Environments

### Dev

```powershell
docker compose -p reservas-dev -f docker-compose.dev.yml up --build -d --force-recreate
```

### QA

```powershell
docker compose -p reservas-qa -f docker-compose.qa.yml up --build -d --force-recreate
```

### Main

```powershell
docker compose -p reservas-main -f docker-compose.main.yml up --build -d --force-recreate
```

## Stop Environments

### Dev

```powershell
docker compose -p reservas-dev -f docker-compose.dev.yml down --remove-orphans
```

### QA

```powershell
docker compose -p reservas-qa -f docker-compose.qa.yml down --remove-orphans
```

### Main

```powershell
docker compose -p reservas-main -f docker-compose.main.yml down --remove-orphans
```

## Logs

### Dev

```powershell
docker compose -p reservas-dev -f docker-compose.dev.yml logs -f
```

### QA

```powershell
docker compose -p reservas-qa -f docker-compose.qa.yml logs -f
```

### Main

```powershell
docker compose -p reservas-main -f docker-compose.main.yml logs -f
```

## Exposed Ports

### Dev

* gateway: `8002`
* ms-auth: `8082`
* ms-reservas: `8092`
* postgres: `5434`
* mongodb: `27018`

### QA

* gateway: `8001`
* ms-auth: `8081`
* ms-reservas: `8091`
* postgres: `5435`
* mongodb: `27019`

### Main

* gateway: `8000`
* ms-auth: `8080`
* ms-reservas: `8090`
* postgres: `5436`
* mongodb: `27020`

## Notes

* Tokens must not be shared between environments.
* The gateway and ms-auth must use the same `JWT_SECRET` inside the same environment.
* This repository currently builds services from sibling folders.
* A future improvement is replacing local `build:` paths with versioned Docker images.

