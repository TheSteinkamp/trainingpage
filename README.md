# TrainingPage Microservice System

## Description 

A fitness application to help you with planning, logging and improving your home training. 
The application has a database of exercises to choose from and also the possibility to add your own exercises.
Main focus is on home training with exercises that don't need any special equipment.
The system is a microservice application built with Spring Boot, React, and PostgreSQL and containerized using Docker for easy setup and deployment.

The `trainingpage` repository acts as the root orchestration repository containing:
- docker-compose configuration
- setup documentation
- environment examples

Each microservice is maintained in its own repository.

## Services

* Frontend (React)
* Gateway Service: The entry point for all client requests. Handles JWT validation.
* Eureka Service Registry: Manages service registration and lookup.
* User Service: Manages authentication and user profiles.
* Training Service: Handles workout logic and exercise data.
* Statistic Service: Processes and generates user performance metrics.
* PostgreSQL databases

## Technology stack

* Java Spring Boot
* React
* PostgreSQL
* Docker & Docker Compose
* JWT Authentication
* OpenFeign
* Eureka Service Discovery

---

# Setup Instructions

### Prerequisites

Docker & Docker Compose
An API Key for the external Exercise API (get it at [https://rapidapi.com/justin-WFnsXH_t6/api/exercisedb])

## 1. Clone the repositories

### Clone the root repo first 

```bash
git clone https://github.com/TheSteinkamp/trainingpage.git
cd trainingpage
```

### Clone the microservices inside the root repo 

```bash
git clone https://github.com/thesteinkamp/trainingpage-user-service.git
git clone https://github.com/thesteinkamp/trainingpage-training-service.git
git clone https://github.com/thesteinkamp/trainingpage-statistic-service.git
git clone https://github.com/thesteinkamp/trainingpage-gateway-service.git
git clone https://github.com/thesteinkamp/trainingpage-service-registry.git
git clone https://github.com/thesteinkamp/trainingpage-frontend.git
```

### Project Structure

After cloning the repositories, the folder structure must look like this:

```txt
trainingpage/
│
├── docker-compose.yml
├── .env
├── user-service/
├── training-service/
├── statistic-service/
├── gateway-service/
├── service-registry/
└── frontend/
```

All repositories must be placed in the same parent directory as the `docker-compose.yml` file.


---

## 3. Create environment variables

Create a `.env` file in the root directory or edit the `.env.example` and add your API key and the enviroment variables as shown in `.env.example`.

---

## 4. Start the system

```bash
docker-compose up --build
```
This will build the images from source and start all services and databases.

---

## Access the application

Once the containers are running:

Open the Eureka Dashboard (http://localhost:8761) and verify that all 4 services (GATEWAY-SERVICE, USER-SERVICE, STATISTIC-SERVICE, GATEWAY-SERVICE) are listed under "Instances currently registered".

Open the Frontend (http://localhost:3000) to start using the application.

---

# Ports

| Service           | Port |
| ----------------- | ---- |
| Frontend          | 3000 |
| Gateway           | 8080 |
| Eureka            | 8761 |
| User Service      | 8081 |
| Statistic Service | 8082 |
| Training Service  | 8083 |
| DB-user			      | 5433 |
| DB-training		    | 5432 |

---

# Architecture

* Frontend communicates only with Gateway.
* Gateway uses Eureka for service discovery.
* Statistic Service communicates with user and training service through OpenFeign.
* User service and training service has their own database and communicates through APIs only.
* The Training Service integrates with the external ExerciseDB API to fetch exercise data.

---

# Authentication

JWT authentication is used for protected routes.
All endpoints (except login/register) require a Bearer JWT token in the Authorization header.

