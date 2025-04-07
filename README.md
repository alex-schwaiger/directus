# Directus Docker Setup

This repository contains a Docker Compose configuration for running [Directus](https://directus.io/), an open-source headless CMS and API, with a PostgreSQL database.

## Project Description

This setup provides a containerized environment for Directus that allows you to quickly deploy and run a Directus instance locally or in a production environment. The configuration includes:

- Directus 11.5.1
- PostgreSQL 13 database
- Persistent storage for database data
- Mounted volumes for uploads and extensions

## Prerequisites

Before getting started, ensure you have the following installed:

- [Docker](https://www.docker.com/get-started) (20.10.0+)
- [Docker Compose](https://docs.docker.com/compose/install/) (v2.0.0+)
- Git (optional, for cloning the repository)

## Installation Steps

1. Clone this repository (or download it directly):
   ```bash
   git clone https://github.com/alex-schwaiger/directus.git
   cd directus
   ```

2. Create a `.env` file in the root directory with the following variables:
   ```
   SECRET=your-random-secret-key
   ADMIN_EMAIL=admin@example.com
   ADMIN_PASSWORD=your-secure-password
   DB_DATABASE=directus
   DB_USER=directus
   DB_PASSWORD=directus-password
   ```

3. Start the containers:
   ```bash
   docker-compose up -d
   ```

## Usage Instructions

Once the containers are up and running:

1. Access the Directus admin interface by navigating to `http://localhost:8055` in your web browser.

2. Log in using the admin credentials you specified in your `.env` file.

3. Start building your content model, managing content, and utilizing the API.

### Common Commands

- Start the containers:
  ```bash
  docker-compose up -d
  ```

- View container logs:
  ```bash
  docker-compose logs -f
  ```

- Stop the containers:
  ```bash
  docker-compose down
  ```

- Stop containers and remove volumes (resets database):
  ```bash
  docker-compose down -v
  ```

## Configuration Options

The setup can be customized by modifying the `docker-compose.yml` file or the environment variables in your `.env` file.

### Key Configuration Options

- **Ports**: By default, Directus runs on port 8055. You can change this by modifying the `ports` section in the `docker-compose.yml` file.

- **Volumes**:
  - `./uploads:/directus/uploads`: Stores uploaded files
  - `./extensions:/directus/extensions`: For custom extensions
  - `postgres_data`: Persistent PostgreSQL database storage

- **Environment Variables**:
  - `SECRET`: Used for securing cookies and tokens
  - `ADMIN_EMAIL` & `ADMIN_PASSWORD`: Credentials for the admin user
  - `DB_*`: Database connection settings
  - `WEBSOCKETS_ENABLED`: Enables real-time functionality

### Adding Extensions

To add custom extensions:

1. Create the appropriate subdirectory in the `./extensions` folder (e.g., `./extensions/interfaces/`)
2. Add your extension files
3. Restart the containers: `docker-compose restart`

## Backup and Restore

### Database Backup
```bash
docker exec -t directus_postgres_1 pg_dump -U your-db-user your-db-name > backup.sql
```

### Database Restore
```bash
cat backup.sql | docker exec -i directus_postgres_1 psql -U your-db-user -d your-db-name
```

## Additional Resources

- [Directus Documentation](https://docs.directus.io/)
- [Directus GitHub Repository](https://github.com/directus/directus)
- [Docker Documentation](https://docs.docker.com/)

