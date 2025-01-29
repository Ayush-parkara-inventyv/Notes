## Overview

Docker is a platform that uses OS-level virtualization to deliver software in packages called containers. Containers bundle an application and its dependencies, ensuring it runs consistently across different environments.

---

## Basics

### Key Terminology

- **Image**: A lightweight, standalone, executable package that includes everything needed to run a piece of software.
- **Container**: A runtime instance of a Docker image.
- **Dockerfile**: A text file with instructions to build a Docker image.
- **Registry**: A storage and distribution system for Docker images (e.g., Docker Hub).

---

## Essential Commands

### Docker Help

```bash
docker --help  # Display help for Docker CLI
docker <command> --help  # Help for a specific command
```

### Pulling Images

```bash
docker pull <image_name>  # Pull the latest version of an image
docker pull <image_name>:<tag>  # Pull a specific version/tag
```

Example:

```bash
docker pull nginx  # Pulls the latest nginx image
docker pull mysql:8.0  # Pulls MySQL version 8.0
```

### Listing Images

```bash
docker images  # List all downloaded images
docker images -a  # Include intermediate images
```

---

## Working with Containers

### Running Containers

```bash
docker run <image_name>  # Run a container from an image
docker run --name <container_name> <image_name>  # Name the container
docker run -d <image_name>  # Run in detached mode
docker run -it <image_name>  # Run interactively (attach terminal)
docker run -p <host_port>:<container_port> <image_name>  # Map ports
```

Example:

```bash
docker run -d -p 8080:80 nginx  # Run nginx, expose port 80 on localhost:8080
```

### Listing Containers

```bash
docker ps  # List running containers
docker ps -a  # List all containers (including stopped ones)
```

### Stopping Containers

```bash
docker stop <container_id or name>  # Stop a running container
docker stop $(docker ps -q)  # Stop all running containers
```

### Removing Containers

```bash
docker rm <container_id or name>  # Remove a stopped container
docker rm $(docker ps -aq)  # Remove all stopped containers
```

---

## Inspecting and Logs

### Inspecting Images and Containers

```bash
docker inspect <container_id or name>  # View detailed info on a container
docker inspect <image_name>  # View detailed info on an image
```

### Viewing Logs

```bash
docker logs <container_id or name>  # View logs of a container
docker logs -f <container_id or name>  # Tail logs (follow output in real-time)
```

---

## Managing Images

### Removing Images

```bash
docker rmi <image_id or name>  # Remove an image
docker rmi $(docker images -q)  # Remove all images
```

### Tagging Images

```bash
docker tag <source_image> <target_image>:<tag>  # Tag an image
```

Example:

```bash
docker tag nginx:latest myrepo/nginx:1.0  # Tag nginx for a private registry
```

### Building Images

```bash
docker build -t <image_name>:<tag> <path_to_dockerfile>  # Build an image
```

Example:

```bash
docker build -t myapp:1.0 .  # Build from the current directory
```

---

## Networking

### Listing Networks

```bash
docker network ls  # List all Docker networks
```

### Creating Networks

```bash
docker network create <network_name>  # Create a custom network
```

### Connecting Containers to Networks

```bash
docker network connect <network_name> <container_name or id>  # Connect a container to a network
```

---

## Docker Compose

Docker Compose simplifies managing multi-container applications.

### Key Commands

```bash
docker-compose up  # Start services defined in docker-compose.yml
docker-compose down  # Stop and remove services
docker-compose ps  # List services and their status
```

Example `docker-compose.yml`:

```yaml
version: "3.8"
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```

---

## Advanced Topics

### Volumes

Volumes store data persistently, even if containers are removed.

#### Commands

```bash
docker volume create <volume_name>  # Create a volume
docker volume ls  # List volumes
docker volume rm <volume_name>  # Remove a volume
```

### Copying Files to/from Containers

```bash
docker cp <source_path> <container_id>:<destination_path>  # Copy to container
docker cp <container_id>:<source_path> <destination_path>  # Copy from container
```

### Pruning Unused Resources

```bash
docker system prune  # Remove unused containers, images, and networks
docker volume prune  # Remove unused volumes
```

---

## References

- [Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- Common troubleshooting tip: Check container logs using `docker logs` when issues arise.

---

> **Pro Tip:** Use `docker system df` to check space usage for images, containers, and volumes!