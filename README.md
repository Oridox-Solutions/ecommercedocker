# E-Commerce Docker Environment

Docker configuration for running the Oridox E-Commerce application locally.

## Services

This repository manages the local development environment using Docker Compose.

Services include:

- Frontend
- Backend
- PostgreSQL Database

## Requirements

- Docker Desktop

## Environment Variables

Create a local environment file:

```bash
cp .env.example .env
```

Update the values with your local PostgreSQL credentials.

Example:

DB_USER=callum
DB_PASSWORD=your_secure_password
DB_NAME=ecommerce

The .env file is ignored by Git and should never be committed.

## Running the Application

Start all services:

```bash
docker compose up --build
```

Stop all services:

```bash
docker compose down
```