# React App with Docker and Docker Compose

This project demonstrates how to dockerize a React application for different environments (development and production) using Docker and Docker Compose.


### Dockerfile.dev

- **Purpose**: Used for the development environment.
- **Features**: Hot-reloading, debugging capabilities.
- **Base Image**: Node.js
- **Port**: 3000

### Dockerfile.prod

- **Purpose**: Used for the production environment.
- **Features**: Optimized build served using Nginx.
- **Base Image**: Node.js (for build), Nginx (for serving)
- **Port**: 80

## Getting Started

### Prerequisites

- **Docker**: Ensure Docker is installed on your machine.
- **Docker Compose**: Docker Compose should also be installed.

### Environment Configuration

The `.env` file is used to manage which Dockerfile is used for building the container.

```plaintext
DOCKERFILE=Dockerfile.dev
```

### Development Environment

1. **Run the app**:
   ```bash
   docker-compose up --build
   ```
   This will build the image using `Dockerfile.dev` and start the app with hot-reloading at `http://localhost:3000`.

2. **Access the app**:
   Open your browser and navigate to `http://localhost:3000`.

### Production Environment

1. **Switch to production**:
   Modify the `.env` file to use the production Dockerfile:
   ```plaintext
   DOCKERFILE=Dockerfile.prod
   ```

2. **Run the app**:
   ```bash
   docker-compose up --build
   ```
   This will build the image using `Dockerfile.prod` and serve the app with Nginx at `http://localhost`.

### Customizing with `docker-compose.override.yml`

You can create a `docker-compose.override.yml` file to override settings, such as volumes and environment variables, for specific environments without modifying the main `docker-compose.yml`.

Example for development:
```yaml
services:
  react-app:
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
```

### Scaling and Extending Services

To add more services (e.g., a PostgreSQL database), include them in the `docker-compose.yml`:

```yaml
services:
  db:
    image: postgres:13
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: react_app_db

volumes:
  pgdata:
```

## Additional Commands

- **Stop containers**:
  ```bash
  docker-compose down
  ```
  Stops and removes containers, networks, volumes, and images created by `up`.

- **Rebuild containers**:
  ```bash
  docker-compose up --build
  ```

- **View logs**:
  ```bash
  docker-compose logs -f
  ```

## Conclusion

By using Docker and Docker Compose, you can easily manage your React application's environments, ensuring consistent and reliable deployments across development, staging, and production.
