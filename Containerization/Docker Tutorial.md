# Docker Tutorial: From Basics to Deployment

## What is Docker?

Docker is a platform that allows you to package applications and their dependencies into lightweight, portable containers. Containers are isolated environments that run consistently across different systems, solving the "it works on my machine" problem.

## Key Concepts

**Image**: A read-only template containing your application and all its dependencies. Think of it as a blueprint.

**Container**: A running instance of an image. Multiple containers can run from the same image.

**Dockerfile**: A text file with instructions to build a Docker image.

**Docker Hub**: A registry where Docker images are stored and shared.

## Installing Docker

Visit [docker.com](https://www.docker.com/products/docker-desktop) and download Docker Desktop for your operating system (Windows, Mac, or Linux).

Verify installation:
```bash
docker --version
docker run hello-world
```

## Basic Docker Commands

### Working with Images

```bash
# Pull an image from Docker Hub
docker pull nginx

# List all images
docker images

# Remove an image
docker rmi nginx
```

### Working with Containers

```bash
# Run a container
docker run nginx

# Run container in detached mode (background)
docker run -d nginx

# Run with a custom name
docker run -d --name my-nginx nginx

# Run and map ports (host:container)
docker run -d -p 8080:80 nginx

# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop a container
docker stop my-nginx

# Start a stopped container
docker start my-nginx

# Remove a container
docker rm my-nginx

# View container logs
docker logs my-nginx

# Execute commands in running container
docker exec -it my-nginx bash
```

## Creating Your First Dockerfile

Let's create a simple Node.js application:

### Step 1: Create Application Files

**app.js**
```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello from Docker!');
});

app.listen(port, () => {
  console.log(`App listening on port ${port}`);
});
```

**package.json**
```json
{
  "name": "docker-app",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.18.0"
  },
  "scripts": {
    "start": "node app.js"
  }
}
```

### Step 2: Create a Dockerfile

**Dockerfile**
```dockerfile
# Use official Node.js runtime as base image
FROM node:18-alpine

# Set working directory in container
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Command to run the application
CMD ["npm", "start"]
```

### Step 3: Build and Run

```bash
# Build the image
docker build -t my-node-app .

# Run the container
docker run -d -p 3000:3000 --name node-container my-node-app

# Access at http://localhost:3000
```

## Dockerfile Best Practices

**Use specific image tags**: Instead of `node:latest`, use `node:18-alpine` for consistency.

**Minimize layers**: Combine RUN commands where possible using `&&`.

**Use .dockerignore**: Exclude unnecessary files (like `node_modules`, `.git`).

**.dockerignore example**
```
node_modules
npm-debug.log
.git
.env
```

**Multi-stage builds**: Reduce final image size by separating build and runtime stages.

```dockerfile
# Build stage
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm install --production
CMD ["node", "dist/index.js"]
```

## Docker Compose

Docker Compose allows you to define and run multi-container applications.

**docker-compose.yml**
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    depends_on:
      - db
    volumes:
      - ./src:/app/src

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_PASSWORD=secret
      - POSTGRES_DB=myapp
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```

**Commands**
```bash
# Start all services
docker-compose up

# Start in detached mode
docker-compose up -d

# Stop all services
docker-compose down

# View logs
docker-compose logs

# Rebuild images
docker-compose build
```

## Volumes and Data Persistence

Containers are ephemeral, but volumes persist data:

```bash
# Create a named volume
docker volume create my-data

# Run container with volume
docker run -d -v my-data:/app/data nginx

# Mount a host directory
docker run -d -v $(pwd)/data:/app/data nginx

# List volumes
docker volume ls

# Remove volume
docker volume rm my-data
```

## Networking

```bash
# Create a network
docker network create my-network

# Run containers on same network
docker run -d --network my-network --name app1 nginx
docker run -d --network my-network --name app2 nginx

# Containers can communicate using service names
```

## Essential Tips

**Clean up resources regularly:**
```bash
# Remove stopped containers
docker container prune

# Remove unused images
docker image prune

# Remove everything unused
docker system prune -a
```

**Check container resource usage:**
```bash
docker stats
```

**Inspect containers and images:**
```bash
docker inspect my-container
docker inspect my-image
```

## Common Use Cases

**Development environment**: Run databases, caches, and services without installing them locally.

**Testing**: Create isolated environments for running tests.

**Production deployment**: Deploy applications consistently across different servers.

**Microservices**: Run multiple services that communicate with each other.

## Next Steps

- Explore Docker Hub for official images
- Learn about container orchestration with Kubernetes
- Study security best practices for production
- Experiment with CI/CD integration
- Try Docker Swarm for container clustering

## Quick Reference Card

| Command | Description |
|---------|-------------|
| `docker run image` | Create and start container |
| `docker ps` | List running containers |
| `docker stop id` | Stop container |
| `docker rm id` | Remove container |
| `docker images` | List images |
| `docker build -t name .` | Build image |
| `docker exec -it id bash` | Access container shell |
| `docker-compose up` | Start services |

Docker transforms how we build, ship, and run applications. Practice these concepts, and you'll quickly become comfortable containerizing your applications.
