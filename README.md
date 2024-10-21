# golang-microservice-simple-starter-template
This is simple golang microservice starter template. Based on simple authentication service, logger service, a test frontend, mailer service with postgreSQL &amp; mongo DB. Building it as my learning project & the project is not completed. Many things still to add.

This starter project implements a Golang-based microservice architecture with multiple services including a broker, logger, listener, authentication, and mailer. The project uses REST, RPC, and gRPC for inter-service communication. The project leverages Docker for containerization and Docker Compose for orchestration.

## Table of Contents

1.  [Project Overview](#project-overview)
2.  [Architecture](#architecture)
3.  [Services Overview](#services-overview)
4.  [Service Communication](#service-communication)
5.  [Technology Stack](#technology-stack)
6.  [Setup and Deployment](#setup-and-deployment)a
7.  [Building and Running](#building-and-running)
8.  [Environment Variables](#environment-variables)
9.  [Future Enhancements](#future-enhancements)

## Project Overview

This project demonstrates a microservice architecture implemented in Golang to gain hands-on experience. It includes:

-   **Frontend Service**: Simple frontend to interact with backend services.
-   **Broker Service**: Acts as a centralized service to route and manage communications.
-   **Logger Service**: Implements both REST and RPC endpoints to manage and store logs.
-   **Listener Service**: Listens to specific events and triggers appropriate actions.
-   **Authentication Service**: Manages basic authentication for users.
-   **Mailer Service**: Handles sending of emails using Mailhog.

The services communicate using REST, gRPC, and RPC protocols, with RabbitMQ serving as the message broker.

## Architecture

The project is designed following a microservice architecture. Below is the architectural flow diagram:

mermaid

Copy code

`graph TD;
  Frontend-->|HTTP REST|Broker;
  Broker-->|RPC/gRPC|Logger;
  Broker-->|HTTP REST|Authentication;
  Broker-->|HTTP REST|Mailer;
  Broker-->|HTTP REST|Listener;
  Mailer-->|SMTP|Mailhog;
  Broker-->|AMQP|RabbitMQ;
  RabbitMQ-->|Listener Events|Listener;
  Logger-->|MongoDB|Mongo;
  Authentication-->|SQL|Postgres;` 

## Services Overview

### 1. **Frontend Service**
-   A basic Golang frontend service to interact with backend services using REST.

### 2. **Broker Service**
-   Routes communication between services.
-   Provides endpoints for triggering inter-service communication.

### 3. **Logger Service**
-   Stores and manages logs.
-   Exposes REST and gRPC interfaces for logging operations.
-   Stores logs in a MongoDB database.

### 4. **Listener Service**
-   Listens to events on RabbitMQ and triggers necessary actions.

### 5. **Authentication Service**
-   Manages user authentication and authorization.
-   Stores user data in a PostgreSQL database.

### 6. **Mailer Service**
-   Handles email operations using Mailhog for local SMTP.

## Service Communication

-   **Broker-Service** acts as a gateway for incoming requests.
-   **Logger-Service** is accessible via REST and RPC.
-   **Mailer-Service** and **Authentication-Service** use RESTful APIs.
-   **Listener-Service** listens to events from RabbitMQ.

## Technology Stack

-   **Golang**: Programming language for building services.
-   **PostgreSQL**: Database for storing user data.
-   **MongoDB**: Database for storing logs.
-   **RabbitMQ**: Message broker for event-driven architecture.
-   **Mailhog**: SMTP server for testing email sending.
-   **Docker**: Containerization of services.
-   **Docker Compose**: Service orchestration.
-   **RPC/gRPC**: Communication protocols for logger-service.

## Setup and Deployment

### Prerequisites

-   Docker and Docker Compose installed.
-   Golang installed (version >= 1.18).

### Deployment Steps
1.  **Clone the Repository**
    `git clone https://github.com/your-repo/project.git
    cd project` 
    
2.  **Build and Start the Services**
    `make up_build` 
    
3.  **Stop Services**
    `make down` 
    
### Docker Compose
The project uses Docker Compose to orchestrate the services. Each service has its own Dockerfile and can be built independently.

### Makefile Commands
-   `make up` - Starts all containers.
-   `make up_build` - Builds and starts all containers.
-   `make down` - Stops all running containers.
-   `make build_[service_name]` - Builds the specified service.

## Environment Variables

Each service has its environment variables specified in `docker-compose.yml`. Important variables include:
-   **Authentication Service**:
    -   `DSN`: Connection string for PostgreSQL.
-   **Mailer Service**:
    -   `MAIL_DOMAIN`, `MAIL_HOST`, `MAIL_PORT`, etc.

## Building and Running

### To Build All Services
`make up_build` 

### To Start Only the Frontend
`make start` 

### To Stop the Frontend
`make stop` 

## Future Enhancements

-   Implement JWT-based authentication.
-   Add centralized logging and monitoring.
-	Further improve & provide advance logging & monitoring. Maybe considering independent centralized logging with ELK stack, structured logging & tracing.
-	Implement JWT-based authentication for stateless interactions.
-   Introduce Kubernetes for orchestration in production environments.
-   Implement CI/CD with GitHub Actions.
-   Add unit and integration testing templates for all services.
-   Add provision for message broker selection between RabbitMQ and Kafka.
-   Provide database configuration for horizontal scaling and clustering.

This list will gradually be updated, as I learn & work on those points, for building scalable microservice starter template.

i had to use absolute path to generate protobuf file
/opt/homebrew/bin/protoc \
  --plugin=protoc-gen-go=$(go env GOPATH)/bin/protoc-gen-go \
  --go_out=. --go_opt=paths=source_relative \
  --plugin=protoc-gen-go-grpc=$(go env GOPATH)/bin/protoc-gen-go-grpc \
  --go-grpc_out=. --go-grpc_opt=paths=source_relative logs.proto