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
- Git
- Access to Repositories

## Development Setup

The Docker development environment uses relative volume mounts to allow hot reloading during development.

The repository structure must remain consistent so Docker can correctly locate the frontend and backend projects.

Expected repository structure:

```text
E-CommerceTemplate/
├── ecommercetemplatebackend/
│   ├── Dockerfile
│   └── package.json
│
├── ecommercetemplatefrontend/
│   ├── Dockerfile
│   └── package.json
│
└── ecommercedocker/
    ├── docker-compose.yml
    └── .env
```

The docker-compose.yml file is located inside:

E-CommerceTemplate/ecommercedocker/

Therefore Docker uses relative paths to access the frontend and backend repositories:

Frontend:
../Frontend/ecommercetemplatefrontend

Backend:
../Backend/ecommercetemplatebackend

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

Clean build:

```bash
docker compose build --no-cache
```

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

Database:

http://localhost:5555

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

PostgreSQL UI:

```bash
docker exec -it ecommerce-backend npx prisma studio
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

Enter the backend container:

```bash
docker exec -it ecommerce-backend sh
```

Enter the frontend container:

```bash
docker exec -it ecommerce-frontend sh
```

Exit a container shell:

```
exit
```

Once inside:

Backend example:

```bash
# inside ecommerce-backend
npm run build
npx prisma migrate status
npx prisma generate
```

Frontend example:

```bash
# inside ecommerce-frontend
npm run build
npm run lint
```

You can also view logs for each service individually:

```bash
docker logs ecommerce-backend
docker logs ecommerce-frontend
docker logs ecommerce-postgres
```


