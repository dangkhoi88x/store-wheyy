# Docker Deployment Guide

This guide explains how to deploy the NumberK Store website using Docker.

## Prerequisites

- Docker installed on your system
- Docker Compose (optional, for easier management)

## Quick Start

### Option 1: Using Docker Compose (Recommended)

1. **Build and start the application:**

   ```bash
   docker-compose up --build
   ```

2. **Run in background:**

   ```bash
   docker-compose up -d --build
   ```

3. **Stop the application:**
   ```bash
   docker-compose down
   ```

### Option 2: Using Docker directly

1. **Build the Docker image:**

   ```bash
   docker build -t numberk-store .
   ```

2. **Run the container:**

   ```bash
   docker run -d -p 80:80 --name numberk-store numberk-store
   ```

3. **Stop and remove the container:**
   ```bash
   docker stop numberk-store
   docker rm numberk-store
   ```

## Access the Application

Once running, the website will be available at:

- http://localhost (if using port 80)
- http://localhost:8080 (if using a different port)

## Production Deployment

### Using Docker Compose

1. **For production deployment, modify the docker-compose.yml:**

   ```yaml
   services:
     web:
       build:
         context: .
         dockerfile: Dockerfile
       ports:
         - "80:80"
         - "443:443" # Add HTTPS port
       restart: always
       environment:
         - NODE_ENV=production
   ```

2. **Deploy:**
   ```bash
   docker-compose -f docker-compose.yml up -d --build
   ```

### Using Docker Swarm

1. **Initialize swarm:**

   ```bash
   docker swarm init
   ```

2. **Deploy stack:**
   ```bash
   docker stack deploy -c docker-compose.yml numberk-store
   ```

## Environment Variables

The application uses the following environment variables:

- `NODE_ENV`: Set to `production` for production builds

## Monitoring

### Health Check

The container includes a health check that verifies the web server is responding:

```bash
docker ps
```

Look for the "healthy" status in the STATUS column.

### Logs

View application logs:

```bash
# Using docker-compose
docker-compose logs -f

# Using docker directly
docker logs -f numberk-store
```

## Troubleshooting

### Common Issues

1. **Port already in use:**

   - Change the port mapping in docker-compose.yml or docker run command
   - Example: `-p 8080:80`

2. **Build fails:**

   - Ensure all files are present in the project directory
   - Check that package.json is valid

3. **Assets not loading:**
   - Verify that all assets are copied correctly in the Dockerfile
   - Check nginx configuration

### Debug Mode

Run the container in interactive mode for debugging:

```bash
docker run -it --rm -p 80:80 numberk-store /bin/sh
```

## File Structure

```
.
├── Dockerfile              # Multi-stage Docker build
├── docker-compose.yml      # Docker Compose configuration
├── nginx.conf             # Nginx server configuration
├── .dockerignore          # Files to ignore during build
└── DEPLOYMENT.md          # This file
```

## Security Considerations

- The nginx configuration includes security headers
- Static assets are cached appropriately
- Consider adding SSL/TLS certificates for production
- Review and update security headers as needed

## Performance Optimization

- Gzip compression is enabled
- Static assets are cached for 1 year
- HTML files are cached for 1 hour
- Video files are optimized for streaming
