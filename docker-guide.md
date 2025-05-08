# Docker: A Comprehensive Guide for Beginners and Interview Preparation

## What is Docker?

Docker is a platform that makes it easier to create, deploy, and run applications by using containers. Containers allow developers to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package.

Think of it like a shipping container - everything needed for your application is packed inside a standardized box that works the same way no matter where you run it.

### Key Benefits of Docker

1. **Consistency**: Applications run the same way everywhere (development, testing, production)
2. **Isolation**: Applications run independently without interfering with each other
3. **Portability**: Easily move applications between different environments
4. **Efficiency**: Containers share the host OS kernel, requiring fewer resources than VMs
5. **Scalability**: Quickly start additional containers when needed
6. **DevOps Integration**: Fits perfectly with CI/CD pipelines and microservices architecture

## Docker Architecture

Docker uses a client-server architecture:

1. **Docker Client**: The command-line interface (CLI) that allows users to interact with Docker
2. **Docker Daemon (Server)**: The background service that builds, runs, and manages containers
3. **Docker Registry**: A storage system for Docker images (like Docker Hub)

The typical workflow goes like this:
- You use the Docker client to issue commands
- The Docker daemon receives these commands and performs the actions
- Images are pulled from or pushed to registries when needed

## Docker Core Concepts Explained

### 1. Docker Engine (In Detail)

The Docker Engine is actually composed of several components:

- **dockerd**: The Docker daemon that manages Docker objects
- **REST API**: An interface for programs to talk to the daemon
- **Docker CLI**: The command-line interface for users

The Docker Engine uses containerd (a container runtime) and runc (a container executor) under the hood to manage containers.

### 2. Images (Expanded)

- **Definition**: Images are read-only templates containing instructions for creating Docker containers.
- **Layers**: Images are composed of multiple layers stacked on top of each other.
  - Each layer represents a set of filesystem changes
  - Layers are cached and reused between images
  - Only modified layers need to be transferred when updating
- **Tags**: Used for versioning (e.g., ubuntu:20.04, nginx:latest)
- **Image IDs**: Unique identifiers for each image

#### Image Naming Convention

```
registry/repository:tag
```
- **Registry**: Where the image is stored (default is Docker Hub)
- **Repository**: The name of the image
- **Tag**: The version (default is 'latest')

Example: `docker.io/nginx:1.19` or just `nginx:1.19` (using default registry)

### 3. Containers (Expanded)

- **Definition**: A container is a runnable instance of an image.
- **Container States**:
  - **Created**: Container created but not started
  - **Running**: Container is running
  - **Paused**: Container execution is paused
  - **Stopped**: Container execution is stopped
  - **Deleted**: Container is removed

- **Container Filesystem**:
  - **Read-only layers**: Come from the image
  - **Writable layer**: Added on top to store container-specific changes
  - **Volumes/Bind mounts**: For persistent storage

- **Container Isolation**:
  - **Namespace isolation**: Process, network, mount, etc.
  - **Control groups (cgroups)**: Resource limits (CPU, memory)

### 4. Dockerfile (Detailed Instructions)

A Dockerfile is a text file with instructions for building a Docker image. Each instruction creates a new layer in the image.

#### Common Instructions (Detailed)

- **FROM**: 
  ```dockerfile
  FROM python:3.9-slim
  ```
  Specifies the base image. Always the first instruction (except for ARG). You can use scratch for a minimal image.

- **WORKDIR**: 
  ```dockerfile
  WORKDIR /app
  ```
  Sets the working directory for subsequent commands. Creates the directory if it doesn't exist.

- **COPY/ADD**: 
  ```dockerfile
  COPY requirements.txt .
  ADD https://example.com/file.tar.gz /tmp/
  ```
  COPY simply copies files from host to container. ADD can also extract archives and download from URLs.

- **RUN**: 
  ```dockerfile
  RUN apt-get update && apt-get install -y wget \
      && rm -rf /var/lib/apt/lists/*
  ```
  Executes commands during build. Combine multiple commands with && to reduce layers.

- **ENV**: 
  ```dockerfile
  ENV NODE_ENV=production PORT=3000
  ```
  Sets environment variables that are available during build and in the container.

- **ARG**: 
  ```dockerfile
  ARG VERSION=latest
  ```
  Defines variables that users can pass at build-time. Only available during build, not in the running container.

- **EXPOSE**: 
  ```dockerfile
  EXPOSE 80/tcp 443/tcp
  ```
  Documents which ports the container listens on. Doesn't actually publish the ports.

- **VOLUME**: 
  ```dockerfile
  VOLUME /data
  ```
  Creates a mount point for persistent data.

- **USER**: 
  ```dockerfile
  USER nobody
  ```
  Sets the user that subsequent commands and the container will run as.

- **ENTRYPOINT**: 
  ```dockerfile
  ENTRYPOINT ["node", "app.js"]
  ```
  The command that always runs when the container starts. Difficult to override.

- **CMD**: 
  ```dockerfile
  CMD ["--help"]
  ```
  Default command or parameters. Can be overridden when running the container.

- **HEALTHCHECK**: 
  ```dockerfile
  HEALTHCHECK --interval=5m --timeout=3s CMD curl -f http://localhost/ || exit 1
  ```
  Instruction to check container health at runtime.

#### Multi-stage Builds

Multi-stage builds allow you to use multiple FROM statements in a Dockerfile, each starting a new build stage. This is useful for creating smaller production images:

```dockerfile
# Build stage
FROM node:14 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

This creates a smaller final image by only including what's necessary for production.

### 5. Docker Registry and Docker Hub (Expanded)

A registry is a service that stores and distributes Docker images. Docker Hub is the default public registry, but you can use others or create private registries.

#### Registry Types:
- **Public**: Docker Hub, GitHub Container Registry, Quay.io
- **Private**: Docker Trusted Registry, AWS ECR, Google Container Registry, Azure Container Registry
- **Self-hosted**: You can run your own registry using the registry image

#### Working with Registries:

```bash
# Log in to a registry
docker login [registry-url]

# Push an image to a registry
docker push username/image:tag

# Pull an image from a registry
docker pull username/image:tag
```

## Docker Commands (Comprehensive List)

### Image Management

```bash
# List local images
docker images
docker image ls

# Search for images on Docker Hub
docker search nginx

# Pull (download) an image
docker pull ubuntu:20.04

# Build an image from a Dockerfile
docker build -t myapp:1.0 .
docker build -f custom.Dockerfile -t myapp:1.0 .

# View image details
docker image inspect image_id

# View image layers
docker history image_id

# Tag an image
docker tag myapp:1.0 myapp:latest
docker tag myapp:1.0 username/myapp:1.0

# Remove images
docker rmi image_id
docker image prune  # Remove unused images
docker image prune -a  # Remove all unused images

# Export/Import images
docker save -o myimage.tar myimage:latest
docker load -i myimage.tar

# Build with build arguments
docker build --build-arg VERSION=1.2.3 -t myapp .
```

### Container Management

```bash
# Create and start a container
docker run -it ubuntu bash
docker run -d nginx  # Detached mode

# List running containers
docker ps
docker container ls

# List all containers (including stopped)
docker ps -a
docker container ls -a

# Start/Stop/Restart containers
docker start container_id
docker stop container_id
docker restart container_id

# Pause/Unpause containers
docker pause container_id
docker unpause container_id

# Execute commands in a running container
docker exec -it container_id bash
docker exec container_id ls -la

# Access logs
docker logs container_id
docker logs -f container_id  # Follow log output
docker logs --tail 100 container_id  # Last 100 lines

# Inspect container details
docker inspect container_id

# Get container stats
docker stats
docker stats container_id

# Remove containers
docker rm container_id
docker rm -f container_id  # Force remove running container
docker container prune  # Remove all stopped containers

# Copy files between host and container
docker cp /host/path container_id:/container/path
docker cp container_id:/container/path /host/path

# View container changes
docker diff container_id

# Create a new image from a container
docker commit container_id new_image_name
```

### Common Docker Run Options (Expanded)

```bash
# Run in detached mode (background)
docker run -d image_name

# Map a port from host to container
docker run -p 8080:80 nginx  # Host port 8080 to container port 80
docker run -p 127.0.0.1:8080:80 nginx  # Bind to specific interface

# Mount a volume (persistent storage)
docker run -v /host/path:/container/path image_name  # Bind mount
docker run -v volume_name:/container/path image_name  # Named volume

# Set environment variables
docker run -e VARIABLE=value image_name
docker run --env-file .env image_name

# Set resource limits
docker run --memory=512m --cpus=0.5 image_name

# Set hostname and DNS
docker run --hostname=webserver --dns=8.8.8.8 image_name

# Add container to a network
docker run --network=my-network image_name

# Auto-remove container when it exits
docker run --rm image_name

# Run with a specific user
docker run --user 1000:1000 image_name

# Set the working directory
docker run --workdir /app image_name

# Mount the Docker socket (for Docker-in-Docker)
docker run -v /var/run/docker.sock:/var/run/docker.sock image_name
```

## Dockerfile Best Practices (With Examples)

### 1. Use Specific Base Images
```dockerfile
# Bad
FROM ubuntu

# Good
FROM ubuntu:20.04
```

### 2. Minimize Layers
```dockerfile
# Bad
RUN apt-get update
RUN apt-get install -y python
RUN apt-get install -y nginx

# Good
RUN apt-get update && \
    apt-get install -y python nginx && \
    rm -rf /var/lib/apt/lists/*
```

### 3. Use .dockerignore
Create a `.dockerignore` file to exclude files from the build context:
```
.git
node_modules
*.log
```

### 4. Order Instructions by Frequency of Change
```dockerfile
# Put instructions that change less frequently first
FROM node:14-alpine
WORKDIR /app

# These rarely change
RUN npm install -g some-tool

# These change more often
COPY package*.json ./
RUN npm install

# These change most frequently
COPY . .
```

### 5. Use Multi-stage Builds
See the multi-stage build example above.

### 6. Use Non-root Users
```dockerfile
FROM python:3.9-slim

# Create a non-root user
RUN useradd -m appuser

# Set the working directory and adjust permissions
WORKDIR /app
COPY --chown=appuser:appuser . .

# Switch to non-root user
USER appuser

CMD ["python", "app.py"]
```

## Docker Networking (In-depth)

Docker creates various network types to allow containers to communicate.

### Network Types

1. **bridge**: The default network. Containers can communicate with each other on the same host.
2. **host**: Removes network isolation between container and host.
3. **none**: No networking for the container.
4. **overlay**: Connect containers across multiple Docker hosts.
5. **macvlan**: Assign a MAC address to a container, making it appear as a physical device.

### Network Commands

```bash
# List networks
docker network ls

# Create a network
docker network create --driver bridge my-network

# Inspect a network
docker network inspect my-network

# Connect a container to a network
docker network connect my-network container_name

# Disconnect a container from a network
docker network disconnect my-network container_name

# Remove a network
docker network rm my-network

# Remove unused networks
docker network prune
```

### Network Example

```bash
# Create a network
docker network create app-network

# Run a database container
docker run -d --name db --network app-network -e POSTGRES_PASSWORD=secret postgres

# Run a web app container
docker run -d --name webapp --network app-network -p 80:5000 mywebapp

# The webapp can now connect to the db container using its name as the hostname
# E.g., postgres://db:5432
```

## Docker Volumes (Detailed)

Volumes are the preferred way to persist data in Docker because they are managed by Docker and independent of the container lifecycle.

### Volume Types

1. **Named volumes**: Created and managed by Docker. Easiest to use.
2. **Bind mounts**: Map a host directory to a container directory.
3. **tmpfs mounts**: Stored in host memory, not on disk.

### Volume Commands

```bash
# List volumes
docker volume ls

# Create a volume
docker volume create my-volume

# Inspect a volume
docker volume inspect my-volume

# Remove a volume
docker volume rm my-volume

# Remove unused volumes
docker volume prune
```

### Volume Examples

```bash
# Using a named volume
docker run -v my-volume:/app/data nginx

# Using a bind mount
docker run -v $(pwd)/logs:/var/log/nginx nginx

# Using a tmpfs mount
docker run --tmpfs /tmp:rw,noexec,nosuid,size=100m nginx
```

## Docker Compose (Detailed)

Docker Compose is a tool for defining and running multi-container Docker applications. Using a YAML file, you can configure all your application's services, networks, and volumes, then start everything with a single command.

### docker-compose.yml Structure

```yaml
version: '3.8'  # Compose file version

services:  # Define your application's services
  web:
    build: ./web  # Path to Dockerfile
    ports:
      - "80:5000"  # Port mapping
    volumes:
      - ./web:/code  # Bind mount
    environment:
      - FLASK_ENV=development  # Environment variables
    depends_on:
      - db  # This service depends on the db service
    restart: always  # Restart policy

  db:
    image: postgres:13
    volumes:
      - db-data:/var/lib/postgresql/data  # Named volume
    environment:
      - POSTGRES_PASSWORD=secret

volumes:  # Define named volumes
  db-data:

networks:  # Define custom networks
  frontend:
  backend:
```

### Docker Compose Commands

```bash
# Start all services (in foreground)
docker-compose up

# Start all services (in background)
docker-compose up -d

# Stop all services
docker-compose down

# Stop all services and remove volumes
docker-compose down -v

# View logs
docker-compose logs

# Follow logs for specific service
docker-compose logs -f web

# View running services
docker-compose ps

# Execute command in a service container
docker-compose exec web bash

# Build or rebuild services
docker-compose build

# Pull the latest images
docker-compose pull

# Scale a service (run multiple instances)
docker-compose up -d --scale web=3
```

### Docker Compose Features

1. **Variable substitution**:
   ```yaml
   services:
     web:
       image: "webapp:${TAG:-latest}"
   ```

2. **Extends and includes** (newer versions use YAML anchors):
   ```yaml
   # Using YAML anchors
   x-common: &common
     restart: always
     environment:
       COMMON_VAR: value

   services:
     web:
       <<: *common  # Include common configuration
       image: nginx
   ```

3. **Healthchecks**:
   ```yaml
   services:
     web:
       image: nginx
       healthcheck:
         test: ["CMD", "curl", "-f", "http://localhost"]
         interval: 30s
         timeout: 10s
         retries: 3
         start_period: 40s
   ```

## Docker Storage and Data Management

### Storage Drivers

Docker uses storage drivers to manage the container's writable layer:

1. **overlay2**: The default and recommended driver for modern Linux distributions
2. **aufs**: Older storage driver used on Ubuntu and Debian
3. **btrfs**: Used when running Docker on Btrfs filesystem
4. **devicemapper**: Used on older versions of CentOS and RHEL
5. **zfs**: Used when running Docker on ZFS
6. **vfs**: Very slow driver that doesn't use any COW mechanism

### Data Persistence Strategies

1. **Volumes**:
   - Docker-managed data storage
   - Best for persistent data
   - Easy backup, migration, and sharing
   
2. **Bind Mounts**:
   - Maps a host file or directory to a container path
   - Good for development (code sharing)
   - Performance depends on host filesystem
   
3. **tmpfs Mounts**:
   - Stores data in host memory only
   - Good for sensitive or temporary data
   - Data is lost when container stops

### Backup and Restore Data

```bash
# Backup a volume
docker run --rm -v my-volume:/source -v $(pwd):/backup alpine tar -czf /backup/my-volume-backup.tar.gz -C /source .

# Restore a volume
docker run --rm -v my-volume:/target -v $(pwd):/backup alpine sh -c "tar -xzf /backup/my-volume-backup.tar.gz -C /target"
```

## Docker Security

### Security Best Practices

1. **Use official images** or trusted sources
2. **Scan images for vulnerabilities**:
   ```bash
   docker scan myimage:latest
   ```
3. **Run containers as non-root users**
4. **Use read-only filesystems when possible**:
   ```bash
   docker run --read-only nginx
   ```
5. **Limit container capabilities**:
   ```bash
   docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx
   ```
6. **Use secrets management** for sensitive data
7. **Set resource limits**
8. **Use security options**:
   ```bash
   docker run --security-opt=no-new-privileges nginx
   ```
9. **Use container isolation technologies**:
   - seccomp: System call filtering
   - AppArmor/SELinux: Mandatory access control

### Docker Content Trust

Sign and verify Docker images:

```bash
# Enable Docker Content Trust
export DOCKER_CONTENT_TRUST=1

# Push signed images
docker push myuser/myimage:latest
```

## Docker Performance Tuning

### Container Resource Limits

```bash
# Limit memory
docker run --memory=2g --memory-swap=2g nginx

# Limit CPU
docker run --cpus=1.5 --cpu-shares=1024 nginx

# Limit I/O
docker run --device-write-bps /dev/sda:1mb nginx
```

### Monitoring Containers

```bash
# Get container stats
docker stats

# Get detailed container info
docker inspect container_id

# Use container monitoring tools
# (cAdvisor, Prometheus, Grafana, etc.)
```

## Advanced Docker Concepts

### Docker Swarm

Docker's native clustering and orchestration solution:

```bash
# Initialize a swarm
docker swarm init

# Join a swarm
docker swarm join --token <token> <manager-ip>:<port>

# Create a service
docker service create --replicas 3 --name my-web nginx

# Scale a service
docker service scale my-web=5

# Update a service
docker service update --image nginx:1.19 my-web
```

### Docker Buildx

Extended build capabilities with BuildKit:

```bash
# Create a new builder instance
docker buildx create --name mybuilder --use

# Build multi-platform images
docker buildx build --platform linux/amd64,linux/arm64 -t myuser/myapp:latest .

# Build and push in one command
docker buildx build --platform linux/amd64,linux/arm64 -t myuser/myapp:latest --push .
```

### Docker Context

Work with multiple Docker hosts:

```bash
# Create a new context
docker context create my-remote --docker "host=ssh://user@remote-host"

# Use a specific context
docker context use my-remote

# List contexts
docker context ls
```

## Interview Questions and Answers (Extended)

### Basic Questions

**Q: What is Docker and why use it?**
A: Docker is a platform that uses OS-level virtualization to deliver software in packages called containers. Containers are isolated from one another and bundle their own software, libraries, and configuration files. Docker solves environment inconsistencies, improves resource utilization, accelerates development cycles, and simplifies deployment.

**Q: What's the difference between Docker and virtual machines?**
A: While both provide isolation, containers share the host OS kernel and are more lightweight (MBs vs GBs), start in seconds (vs minutes), and use fewer resources. VMs include a full OS copy for each instance and have stronger isolation because they don't share the kernel.

**Q: Explain Docker images and containers.**
A: An image is a read-only template with instructions for creating a Docker container. It includes the code, runtime, libraries, environment variables, and config files. A container is a runnable instance of an image—what the image becomes in memory when executed. You can create, start, stop, move, or delete containers using the Docker API or CLI.

**Q: How does Docker work under the hood?**
A: Docker uses several Linux kernel features like namespaces (for isolation), control groups (for resource limiting), union file systems (for layered images), and container format (combining these components). The Docker Engine executes commands, while containerd manages the container lifecycle, and runc actually runs the containers.

### Intermediate Questions

**Q: What is a Docker layer? How do layers work?**
A: Docker images are built in read-only layers, with each instruction in a Dockerfile creating a new layer. Each layer only stores the differences from the previous layer. When a container runs, a writable layer is added on top. Layers are cached and reused, making builds faster and more efficient. Only modified layers need to be rebuilt when updating an image.

**Q: What is Docker Compose and when would you use it?**
A: Docker Compose is a tool for defining and running multi-container Docker applications using a YAML file. You'd use it when your application requires multiple interrelated services (like a web server, database, and cache), as it simplifies the process of setting up and managing these services together.

**Q: How do you persist data in Docker?**
A: Docker offers three main options for data persistence: volumes (Docker-managed directories on the host filesystem), bind mounts (mapping host directories to container paths), and tmpfs mounts (storing data in host memory). Volumes are the preferred mechanism because they're independent of the container lifecycle and can be easily backed up and shared.

**Q: How do containers communicate with each other?**
A: Containers can communicate through Docker networks. By default, containers on the same bridge network can reach each other using container names as hostnames. For more complex scenarios, you can create custom networks, use overlay networks for multi-host communication, or expose ports to allow external access.

### Advanced Questions

**Q: How would you implement CI/CD with Docker?**
A: I would create a pipeline that:
1. Builds Docker images from application source code
2. Runs automated tests inside containers
3. Scans images for vulnerabilities
4. Tags images with version/build numbers
5. Pushes images to a container registry
6. Deploys containers to development/staging/production environments
7. Includes rollback mechanisms for failed deployments

**Q: How would you scale a Dockerized application?**
A: Scaling can be achieved through:
1. **Horizontal scaling**: Running multiple container instances behind a load balancer
2. **Orchestration**: Using Docker Swarm or Kubernetes to manage container scaling
3. **Stateless design**: Ensuring containers can be started/stopped without data loss
4. **Service discovery**: Allowing containers to find and communicate with each other
5. **Distributed storage**: Using shared volumes or external databases for persistent data

**Q: What security measures would you implement for Docker in production?**
A: I would:
1. Use minimal base images to reduce attack surface
2. Run containers as non-root users
3. Implement image scanning in the CI/CD pipeline
4. Use Docker Content Trust for image signing
5. Apply resource limits to prevent DoS attacks
6. Configure network segmentation
7. Use secrets management for sensitive data
8. Implement logging and monitoring
9. Regularly update images and apply security patches
10. Use security profiles (AppArmor, SELinux, seccomp)

**Q: How would you troubleshoot a failing Docker container?**
A: My troubleshooting approach would be:
1. Check container logs: `docker logs container_id`
2. Inspect container details: `docker inspect container_id`
3. Check resource usage: `docker stats container_id`
4. Try executing commands inside the container: `docker exec -it container_id bash`
5. Check application logs within the container
6. Verify network connectivity with other services
7. Validate volume mounts and file permissions
8. Review container startup commands and environment variables
9. Compare with a working container or try with a fresh container

### Docker in Production Questions

**Q: How do you manage Docker container logs in production?**
A: In production, I would:
1. Configure the Docker logging driver (json-file, syslog, fluentd, etc.)
2. Set log rotation policies to prevent disk space issues
3. Use a centralized logging stack (ELK, Graylog, Splunk)
4. Implement structured logging in applications
5. Set up log monitoring and alerting

**Q: How would you handle Docker container orchestration?**
A: For orchestration, I would use:
1. **Kubernetes**: For complex, large-scale deployments
2. **Docker Swarm**: For simpler deployments that need clustering
3. **Amazon ECS/EKS**: For AWS environments
4. **Azure AKS**: For Azure environments
5. **Google GKE**: For Google Cloud environments

**Q: How do you ensure high availability of containerized applications?**
A: To ensure high availability:
1. Deploy multiple container instances across different hosts
2. Use health checks to detect and replace failing containers
3. Implement load balancing across container instances
4. Use orchestration platforms with self-healing capabilities
5. Design for statelessness where possible
6. Set up proper monitoring and alerting
7. Implement automated rollbacks for failed deployments
8. Use a proper disaster recovery strategy

## Docker Best Practices (Extended)

1. **Create ephemeral containers**: Containers should be disposable and easily replaceable.

2. **Follow the single responsibility principle**: Each container should have only one concern.

3. **Minimize the number of layers**: Combine commands to reduce the image size.

4. **Use multi-stage builds**: Separate build-time and runtime dependencies.

5. **Don't install unnecessary packages**: Keep containers minimal.

6. **Use .dockerignore files**: Exclude unnecessary files from the build context.

7. **Use environment variables for configuration**: Make containers reusable across environments.

8. **Use specific tags instead of latest**: Ensure reproducible builds.

9. **Use health checks**: Monitor container health at runtime.

10. **Set resource limits**: Prevent containers from consuming too many resources.

11. **Use non-root users**: Improve security by avoiding root privileges.

12. **Use proper restart policies**: Ensure containers recover from failures.

13. **Label your images**: Add metadata about owners, versions, etc.

14. **Scan images for vulnerabilities**: Prevent security issues.

15. **Document your Dockerfiles**: Help others understand your images.

16. **Use version control for Dockerfiles**: Track changes over time.

17. **Use build caches efficiently**: Order Dockerfile instructions to maximize cache usage.

18. **Keep images small**: Smaller images are faster to deploy and have fewer vulnerabilities.

19. **Use proper networking**: Isolate containers appropriately.

20. **Back up volumes**: Ensure data persistence and recovery.
