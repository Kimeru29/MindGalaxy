---
Title: Introduction to Docker
Tags: #Docker #containerization #DevOps
---

# Introduction to Docker

## What is Docker?

Docker is an open-source platform that automates the deployment, scaling, and management of applications in containers. It allows developers to package their applications and dependencies into a lightweight, portable container that can run consistently on any environment.

## Why use Docker?

- **Consistency**: Containers provide a consistent environment for applications, ensuring they run the same way in development, testing, and production.
- **Portability**: Docker containers can run on any platform that supports Docker, making it easy to move applications between environments.
- **Isolation**: Containers are isolated from each other and the host system, reducing conflicts and improving security.
- **Scalability**: Docker makes it easy to scale applications horizontally by adding more containers.

## Key Docker Components

- **Docker Engine**: The core runtime component responsible for building, running, and managing containers.
- **Dockerfile**: A text file containing instructions for building a Docker image.
- **Docker Image**: A snapshot of an application and its dependencies, used to create containers.
- **Docker Container**: A running instance of a Docker image.
- **Docker Hub**: A public registry for storing and sharing Docker images.

## Basic Docker Commands

- `docker build`: Build a Docker image from a Dockerfile.
- `docker run`: Create and run a container from a Docker image.
- `docker ps`: List running containers.
- `docker stop`: Stop a running container.
- `docker rm`: Remove a stopped container.

## Related Notes

- [[Docker Compose]]
- [[Docker Swarm]]
- [[Kubernetes]]
