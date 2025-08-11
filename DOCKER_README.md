# Docker Setup for Strapi Jobs

This document explains how to build and run the Strapi application using Docker.

## Prerequisites

- Docker installed on your system
- Docker Compose installed

## Quick Start

### 1. Build and Run with Docker Compose

```bash
# Build and start all services
docker-compose up --build

# Run in background
docker-compose up -d --build
```

### 2. Build Docker Image Only

```bash
# Build the image
docker build -t strapi-jobs .

# Run the container
docker run -p 1337:1337 -e DATABASE_CLIENT=sqlite strapi-jobs
```

## Environment Variables

Create a `.env` file in the root directory with these variables:

```bash
# Database Configuration
DATABASE_CLIENT=postgres
DATABASE_HOST=postgres
DATABASE_PORT=5432
DATABASE_NAME=strapi
DATABASE_USERNAME=strapi
DATABASE_PASSWORD=strapi123
DATABASE_SSL=false

# Strapi Configuration
NODE_ENV=production
PORT=1337
HOST=0.0.0.0

# Security Keys (Generate these for production)
APP_KEYS=your-app-keys-here,replace-with-your-keys
API_TOKEN_SALT=your-api-token-salt-here
ADMIN_JWT_SECRET=your-admin-jwt-secret-here
JWT_SECRET=your-jwt-secret-here
TRANSFER_TOKEN_SALT=your-transfer-token-salt-here
```

## Generate Security Keys

For production, generate secure random keys:

```bash
# Generate APP_KEYS (comma-separated)
openssl rand -base64 32
openssl rand -base64 32
openssl rand -base64 32
openssl rand -base64 32

# Generate other secrets
openssl rand -base64 32
```

## Docker Commands

```bash
# View logs
docker-compose logs -f strapi

# Stop services
docker-compose down

# Stop and remove volumes
docker-compose down -v

# Rebuild and restart
docker-compose up --build --force-recreate

# Access container shell
docker exec -it strapi-jobs sh
```

## Access Points

- **Strapi Admin**: http://localhost:1337/admin
- **API**: http://localhost:1337/api
- **Database**: localhost:5432

## Troubleshooting

### Port Already in Use
If port 1337 is already in use, change it in docker-compose.yml:
```yaml
ports:
  - "1338:1337"  # Use port 1338 on host
```

### Database Connection Issues
Ensure PostgreSQL container is running:
```bash
docker-compose ps
docker-compose logs postgres
```

### Permission Issues
If you encounter permission issues with uploads:
```bash
docker exec -it strapi-jobs chown -R strapi:nodejs /app/public/uploads
```

## Production Considerations

1. **Use proper secrets management** (not hardcoded in docker-compose.yml)
2. **Set up proper SSL/TLS certificates**
3. **Configure backup strategies for database and uploads**
4. **Use environment-specific configurations**
5. **Set up monitoring and logging** 