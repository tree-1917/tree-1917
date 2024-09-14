---
title: 📦 Docker Tutorial
---

# 📦 Docker: Introduction Tutorial

Welcome to the Docker introduction tutorial! Docker is a powerful platform designed to automate the deployment, scaling, and management of applications using containerization. In this tutorial, we'll cover the basics of Docker, including installation, key features, and basic commands to get you started.

## 📑 Table of Contents

1. [What is Docker?](#what-is-docker)
2. [Key Features](#key-features)
3. [Installation](#installation)
4. [Basic Commands](#basic-commands)
5. [Getting Started with Docker](#getting-started-with-docker)
6. [Further Reading](#further-reading)

## 🤔 What is Docker?

Docker is a containerization platform that allows developers to package applications and their dependencies into containers. Containers are lightweight, portable, and provide a consistent environment for running applications across different systems.

By using Docker, you can ensure that your application runs the same way in development, testing, and production environments, regardless of the underlying system.

## 🌟 Key Features

- **Isolation**: Containers run in isolated environments, ensuring that applications do not interfere with each other.
- **Portability**: Docker containers can run on any system that supports Docker, making it easy to move applications between environments.
- **Efficiency**: Containers share the host OS kernel, making them more efficient than traditional virtual machines.
- **Version Control**: Docker images can be versioned and managed through Docker Hub or private registries.

## 🛠️ Installation

### For Ubuntu/Linux:

1. **Update Package Index**:

   ```bash
   sudo apt-get update
   ```

2. **Install Required Packages**:

   ```bash
   sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
   ```

3. **Add Docker’s Official GPG Key**:

   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

4. **Set up the Docker Repository**:

   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```

5. **Install Docker Engine**:

   ```bash
   sudo apt-get update
   sudo apt-get install docker-ce
   ```

6. **Verify Docker Installation**:
   ```bash
   docker --version
   ```

### For Windows/Mac:

- Download and install Docker Desktop from the [Docker website](https://www.docker.com/products/docker-desktop).
- Follow the installation instructions for your operating system.

## 💻 Basic Commands

- **Verify Docker Installation**:

  ```bash
  docker --version
  ```

- **Run a Sample Container**:

  ```bash
  docker run hello-world
  ```

  This command pulls the `hello-world` image from Docker Hub and runs it in a container, printing a confirmation message.

- **List Running Containers**:

  ```bash
  docker ps
  ```

- **List All Containers (including stopped)**:

  ```bash
  docker ps -a
  ```

- **Stop a Running Container**:

  ```bash
  docker stop <container_id>
  ```

- **Remove a Container**:
  ```bash
  docker rm <container_id>
  ```

## 🚀 Getting Started with Docker

1. **Create a Dockerfile**:
   A Dockerfile is a text document that contains all the commands to build a Docker image. Here’s a simple example:

   ```Dockerfile
   # Use an official Python runtime as a parent image
   FROM python:3.8-slim

   # Set the working directory in the container
   WORKDIR /app

   # Copy the current directory contents into the container at /app
   COPY . /app

   # Install any needed packages specified in requirements.txt
   RUN pip install --no-cache-dir -r requirements.txt

   # Make port 80 available to the world outside this container
   EXPOSE 80

   # Define environment variable
   ENV NAME World

   # Run app.py when the container launches
   CMD ["python", "app.py"]
   ```

2. **Build an Image**:

   ```bash
   docker build -t my-python-app .
   ```

3. **Run a Container**:
   ```bash
   docker run -p 4000:80 my-python-app
   ```

## 📚 Further Reading

- [Docker Official Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Docker CLI Cheat Sheet](https://docs.docker.com/engine/reference/commandline/docker/)

---
