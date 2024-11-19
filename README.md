
# CBD3324-ICA2

This repository contains the solution for **Individual Coursework Assignment 2 (ICA2)** for the **CBD3324** module. The project focuses on deploying a multi-container application into a Kubernetes (K8s) cluster using best practices for manifest creation and secret management.

## Project Overview

The goal of this project is to deploy and configure a multi-container application on a real Kubernetes cluster (not minikube or Docker Desktop). The application includes:

1. **Flask Web Application** (Frontend)  
   - Connects to a PostgreSQL database to retrieve user data.  
   - Uses Redis as a caching service for performance improvement.

2. **PostgreSQL Database** (Backend)  
   - Stores user data.

3. **Redis Cache**  
   - Caches frequently accessed data to improve application performance.

## Components and Technologies

### Flask Application
- Base image: `python:3.9-slim`
- Includes the following packages:
  - `Flask==2.0.1`
  - `psycopg2-binary==2.9.1`
  - `redis==3.5.3`
  - `Werkzeug==2.2.2`
- Environment variables for configuration:
  - `PGDB_HOST`, `PGDB_USER`, `PGDB_PASSWORD`, `PGDB_NAME` for PostgreSQL.
  - `REDIS_HOST` for Redis.

### PostgreSQL Database
- Image: `postgres:13`
- Runs on port `5432`.
- Configured using environment variables:
  - `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`.

### Redis Cache
- Image: `redis:latest`.

## Key Tasks

1. **Namespaces**
   - Created three namespaces, one for each component: Flask, PostgreSQL, and Redis.

2. **Kubernetes Manifests**
   - Deployment, Service, ConfigMap, and Secrets manifests were created for each component.
   - Secrets were encrypted using **SOPS** before committing to the repository.

3. **Database Setup**
   - After deploying PostgreSQL, the following commands were used:
     ```bash
     kubectl exec -it <postgres-pod-name> -n <namespace> -- bash
     psql -h localhost -U <username> -d <database-name>
     ```
   - SQL commands executed:
     ```sql
     CREATE TABLE users (ID INT PRIMARY KEY NOT NULL, NAME TEXT NOT NULL);
     INSERT INTO users VALUES (1, '3324_1');
     ```

4. **Secret Management**
   - Used **SOPS** for encrypting the `secrets.yaml` file.

## Setup Instructions

1. Clone this repository.
   ```bash
   git clone https://github.com/bamnelkarvivek/CBD3324-ICA2.git
   cd CBD3324-ICA2
   ```

2. Deploy the application to your K8s cluster:
   - Apply namespaces:
     ```bash
     kubectl apply -f namespaces.yaml
     ```
   - Apply manifests for each component:
     ```bash
     kubectl apply -f flask/
     kubectl apply -f postgres/
     kubectl apply -f redis/
     ```

3. Verify all components are running:
   ```bash
   kubectl get pods -A
   ```

4. Access the Flask application via the service IP or Ingress (if configured).

## Deliverables

- Word/PDF report including:
  - Commands and outputs (with snapshots).
  - Explanation of work done.
- Repository with all manifest files and encrypted secrets.
