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
../ecommercetemplatefrontend

Backend:
../ecommercetemplatebackend

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

Prisma Studio (Database UI):

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

Prisma Studio (Database UI):

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

## Troubleshooting

### Docker Containers Will Not Start

Check the status of the containers:

```bash
docker compose ps
```

View the container logs for more information:

```bash
docker compose logs
```

You can also view logs for an individual service:

```bash
docker logs ecommerce-backend
docker logs ecommerce-frontend
docker logs ecommerce-postgres
```

If the containers or images are in an unexpected state, rebuild the application:

```bash
docker compose down
docker compose up --build
```

### Port Already in Use

If Docker reports that a port is already in use, check which process is using the port.

The application uses:

* `5173` for the frontend
* `3000` for the backend
* `5432` for PostgreSQL
* `5555` for Prisma Studio

Stop the application or service currently using the required port before starting Docker again.

### Database Connection Errors

If the backend cannot connect to PostgreSQL, first check that the database container is running:

```bash
docker compose ps
```

Check the PostgreSQL logs:

```bash
docker logs ecommerce-postgres
```

Verify that the database credentials in `.env` match the values configured in `docker-compose.yml`:

```env
DB_USER=your_username
DB_PASSWORD=your_secure_password
DB_NAME=ecommerce
```

The backend connects to PostgreSQL using the Docker service name `db`, rather than `localhost`.

### Prisma Migration Errors

Check the current migration status:

```bash
docker exec -it ecommerce-backend npx prisma migrate status
```

If migrations have not been applied, run:

```bash
docker exec -it ecommerce-backend npx prisma migrate deploy
```

If the local database can be safely reset, remove the Docker database volume and recreate the services:

```bash
docker compose down -v
docker compose up --build -d
```

Warning: resetting the database permanently removes all local database data.

### Environment Variable Problems

Make sure a local `.env` file exists in the Docker repository:

```bash
cp .env.example .env
```

Check that the required variables are defined:

```env
DB_USER=your_username
DB_PASSWORD=your_secure_password
DB_NAME=ecommerce
LOG_LEVEL=log
```

Do not commit the `.env` file to Git.

### Frontend or Backend Changes Are Not Updating

The development containers use volume mounts to enable hot reloading.

If changes are not being detected, check that the containers are running:

```bash
docker compose ps
```

Restart the development environment:

```bash
docker compose down
docker compose up --build
```

If the problem persists, perform a clean rebuild:

```bash
docker compose down
docker compose build --no-cache
docker compose up
```

### Application Is Running but an Endpoint Does Not Work

Check that the backend is running:

```bash
docker logs ecommerce-backend
```

Verify the backend health endpoint:

```text
http://localhost:3000/health
```

If the backend is running but an endpoint returns an error, check the backend logs for the corresponding request and error message.

### Complete Docker Reset

If the development environment is in an unrecoverable state, perform a complete local reset:

```bash
docker compose down -v
docker compose up --build -d
```

This removes the containers and PostgreSQL volume and recreates the development environment from scratch.

Warning: all local PostgreSQL data will be deleted.
