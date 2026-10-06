# Docker Notes for DevOps Engineers

---

# Architecture

Docker follows a Client-Server Architecture.

```text
Docker Client
     |
     v
Docker Daemon
     |
     v
Images -> Containers -> Networks -> Volumes
```

---

# Dockerfile Workflow

```text
Dockerfile
    |
    v
docker build
    |
    v
Docker Image
    |
    v
docker run
    |
    v
Container
```

---

# Common Dockerfile Instructions

| Instruction | Description |
|------------|-------------|
| FROM | Defines the base image |
| RUN | Executes commands during image build |
| COPY | Copies files from host machine into image |
| ADD | Automatically extracts archives and can handle URLs |
| WORKDIR | Sets working directory |
| CMD | Defines default command when container starts (only one CMD should exist) |
| ENTRYPOINT | Forces container to run a particular command and is difficult to override |
| ENV | Sets environment variables |
| EXPOSE | Documents container port |
| USER | Defines which user runs the container |
| VOLUME | Creates persistent storage |

---

# Multi-Stage Dockerfile

Multi-stage Docker builds use multiple `FROM` statements.

The first stage builds the application, and the final stage copies only the required artifacts, resulting in:

- Smaller images
- Better security
- Faster deployment
- Cleaner Dockerfiles

## Why Use Multi-Stage Builds?

- Smaller images
- Better security
- Faster deployment
- Cleaner Dockerfiles

---

# Distroless Images

Distroless images are minimal Docker images that contain only the application and required runtime dependencies, without operating system utilities such as:

- bash
- apt
- yum
- curl
- wget

## Benefits

- Smaller image size
- Better security
- Faster deployments
- Reduced attack surface

A Distroless Image contains:

✅ Application

✅ Required Runtime (Java, Python, Node.js, etc.)

❌ No bash or sh

❌ No apt, yum, apk

❌ No curl, wget, vim

---

# Alpine vs Distroless

| Feature | Alpine | Distroless |
|----------|---------|------------|
| Shell Available | ✅ Yes | ❌ No |
| Package Manager | ✅ apk | ❌ No |
| Easy Debugging | ✅ Yes | ❌ Hard |
| Image Size | Small | Smaller |
| Security | Good | Better |

---

# Docker Bind Mount

A Bind Mount maps a file or directory from the host machine directly into a Docker container.

```text
Host Directory/File
         |
         v
Docker Container
```

Benefits:

- Real-time file sharing
- Great for development
- Easy configuration management

Example:

```bash
docker run -v /home/user/data:/app/data nginx
```

---

# Docker Volume

A Docker Volume is a persistent storage mechanism that stores data outside the container lifecycle.

Data remains available even after the container is deleted.

## Types of Docker Volumes

1. Named Volume
2. Bind Mount
3. Anonymous Volume

---

## Docker Volume Comparison

| Type | Managed By | Example |
|--------|-----------|----------|
| Named Volume | Docker | myvolume:/data |
| Bind Mount | User | /home/rke/data:/data |
| Anonymous Volume | Docker (random name) | /data |

---

## Volume Commands

| Command | Description |
|----------|-------------|
| docker volume create myvolume | Creates a volume |
| docker volume ls | Lists all volumes |
| docker volume inspect myvolume | Shows volume details (metadata, mount path, driver) |
| docker volume rm myvolume | Removes a specific volume |
| docker volume prune | Removes unused volumes |

---

# Backup of Docker Volume

A common method is creating a TAR backup.

```bash
docker run --rm \
-v myvolume:/source \
-v $(pwd):/backup \
ubuntu \
tar czvf /backup/myvolume-backup.tar.gz -C /source .
```

---

# Docker Network

Docker networking allows communication between:

- Containers
- Host Machine
- External Systems

## Types of Docker Networks

| Network Type | Description |
|--------------|-------------|
| Bridge | Default network. Container gets its own IP |
| Host | Shares host network |
| None | No network connectivity |
| Overlay | Communication across multiple hosts |
| Macvlan | Real physical network IP |

---

## Docker Network Commands

| Command | Description |
|----------|-------------|
| docker network ls | List networks |
| docker network create mynet | Create network |
| docker network inspect mynet | View network details |
| docker network rm mynet | Remove network |
| docker network connect mynet container1 | Connect container to network |
| docker network disconnect mynet container1 | Disconnect container from network |

---

# Docker Commands Cheat Sheet

## Image Commands

| Command | Explanation |
|----------|-------------|
| docker --version | Shows Docker version |
| docker info | Displays detailed Docker information |
| docker images | Lists all Docker images |
| docker image ls | Lists Docker images |
| docker pull nginx | Downloads an image from Docker Hub |
| docker build -t app:v1 . | Builds an image from a Dockerfile |
| docker rmi nginx | Removes an image |
| docker rmi -f nginx | Force removes an image |

---

## Container Commands

| Command | Explanation |
|----------|-------------|
| docker run nginx | Creates and starts a container |
| docker run -d nginx | Runs a container in background |
| docker run -it ubuntu bash | Runs an interactive container |
| docker run --name web nginx | Creates a container with custom name |
| docker run -p 8080:80 nginx | Maps host port to container port |
| docker ps | Lists running containers |
| docker ps -a | Lists all containers |
| docker stop <container> | Stops a running container |
| docker start <container> | Starts a stopped container |
| docker restart <container> | Restarts a container |
| docker rm <container> | Removes a container |
| docker rm -f <container> | Force removes a running container |

---

## Container Inspection Commands

| Command | Explanation |
|----------|-------------|
| docker inspect <container> | Displays detailed container information |
| docker top <container> | Shows running processes inside container |
| docker diff <container> | Shows file changes inside container |
| docker port <container> | Displays port mappings |
| docker stats | Displays CPU, Memory and Network usage |
| docker stats <container> | Resource usage of a specific container |

---

## Logs & Troubleshooting

| Command | Explanation |
|----------|-------------|
| docker logs <container> | Shows container logs |
| docker logs -f <container> | Streams logs continuously |
| docker logs --tail 100 <container> | Last 100 log lines |
| docker exec -it <container> bash | Open bash shell |
| docker exec -it <container> sh | Open sh shell |
| docker exec <container> ls -l | Execute command inside container |

---

## Docker Volume Commands

| Command | Explanation |
|----------|-------------|
| docker volume create myvolume | Creates a volume |
| docker volume ls | Lists all volumes |
| docker volume inspect myvolume | Shows volume details |
| docker volume rm myvolume | Removes a volume |
| docker volume prune | Deletes unused volumes |
| docker volume prune -f | Force deletes unused volumes |

---

## Docker Copy Commands

| Command | Explanation |
|----------|-------------|
| docker cp file.txt container:/tmp/ | Copy file from host to container |
| docker cp container:/tmp/file.txt . | Copy file from container to host |

---

## Image Save & Load Commands

| Command | Explanation |
|----------|-------------|
| docker save nginx > nginx.tar | Saves image as TAR |
| docker load < nginx.tar | Loads image from TAR |
| docker export <container> > cont.tar | Exports container filesystem |
| docker import cont.tar app:v1 | Creates image from exported container |

---

## Docker Registry Commands

| Command | Explanation |
|----------|-------------|
| docker login | Login to Docker registry |
| docker logout | Logout from Docker registry |
| docker tag app:v1 repo/app:v1 | Tags image |
| docker push repo/app:v1 | Pushes image to registry |
| docker pull repo/app:v1 | Pulls image from registry |

---

## Docker Cleanup Commands

| Command | Explanation |
|----------|-------------|
| docker container prune | Removes stopped containers |
| docker image prune | Removes unused images |
| docker network prune | Removes unused networks |
| docker volume prune | Removes unused volumes |
| docker system prune | Removes unused Docker resources |
| docker system prune -a | Removes all unused resources including images |

---

## Docker History Commands

| Command | Explanation |
|----------|-------------|
| docker history nginx | Shows image layers |
| docker events | Shows Docker events in real-time |
| docker system df | Shows Docker disk usage |

---

# Quick Interview Revision

| Category | Important Commands |
|------------|-------------------|
| Images | pull, build, images, rmi |
| Containers | run, ps, stop, start, rm |
| Logs | logs, exec |
| Volumes | volume create, volume ls, volume inspect |
| Networks | network create, network ls, network inspect |
| Troubleshooting | inspect, stats, history, top |
| Cleanup | prune, system prune -a |

---

# Key Interview Points

✅ Default Docker Network: **Bridge**

✅ Default Container Storage: **Writable Layer**

✅ Volume Types:
- Named Volume
- Bind Mount
- Anonymous Volume

✅ Multi-stage Builds:
- Smaller Images
- Better Security
- Faster Deployment

✅ Distroless Images:
- No Shell
- No Package Manager
- Production Ready
- Highly Secure
