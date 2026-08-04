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

Build and start all services:

```bash
docker compose up --build
```

Start existing containers:

```bash
docker compose up -d
```

Stop all services:

```bash
docker compose down
```

Reset the database (removes all local data):

```bash
docker compose down -v
docker compose up --build -d
```

The PostgreSQL database container is created automatically by Docker using the values defined in `.env`.

## Application URLs

Frontend:

http://localhost:5173

Backend:

http://localhost:3000

Health check:

http://localhost:3000/health

## Useful Docker Commands

View running containers:

```bash
docker ps
```

View container logs:

```bash
docker compose logs
```

Follow live logs:

```bash
docker compose logs -f
```

Enter the PostgreSQL shell:

```bash
docker exec -it ecommerce-postgres psql -U <username> -d ecommerce
```

List databases:

```sql
\l
```

List database users:

```sql
\du
```

Exit PostgreSQL:

```sql
\q
```
