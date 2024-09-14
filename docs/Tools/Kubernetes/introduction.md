---
title: ☸️ Kubernetes Introduction
---

# ☸️ Kubernetes: Introduction Tutorial

Welcome to the Kubernetes introduction tutorial! Kubernetes is a powerful open-source platform designed to automate the deployment, scaling, and management of containerized applications. In this guide, we'll cover the fundamentals of Kubernetes, including its key features, installation, and basic commands to help you get started.

## 📑 Table of Contents

1. [What is Kubernetes?](#what-is-kubernetes)
2. [Key Features](#key-features)
3. [Installation](#installation)
4. [Basic Commands](#basic-commands)
5. [Getting Started with Kubernetes](#getting-started-with-kubernetes)
6. [Further Reading](#further-reading)

## 🤔 What is Kubernetes?

Kubernetes (often abbreviated as K8s) is an open-source container orchestration platform that simplifies the management of containerized applications across clusters of machines. It automates many tasks involved in deploying and managing applications, including scaling, load balancing, and failover.

## 🌟 Key Features

- **Automated Scaling**: Adjusts the number of running containers based on demand.
- **Self-Healing**: Replaces failed containers and restarts them as necessary.
- **Service Discovery and Load Balancing**: Automatically exposes services and manages load balancing.
- **Storage Orchestration**: Manages storage resources for containers, allowing them to use storage solutions easily.

## 🛠️ Installation

### For Linux (Using Minikube)

1. **Install Minikube**:

   ```bash
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   sudo install minikube-linux-amd64 /usr/local/bin/minikube
   ```

2. **Start Minikube**:

   ```bash
   minikube start
   ```

3. **Verify Kubernetes Cluster**:
   ```bash
   kubectl cluster-info
   ```

### For Windows/Mac

- Download and install Minikube from the [Minikube GitHub releases page](https://github.com/kubernetes/minikube/releases).
- Follow the installation instructions specific to your operating system.

## 💻 Basic Commands

- **Check Cluster Information**:

  ```bash
  kubectl cluster-info
  ```

- **List Nodes in the Cluster**:

  ```bash
  kubectl get nodes
  ```

- **Create a Deployment**:

  ```bash
  kubectl create deployment my-deployment --image=nginx
  ```

- **Expose a Deployment**:

  ```bash
  kubectl expose deployment my-deployment --port=80 --target-port=80 --type=NodePort
  ```

- **View Pods**:

  ```bash
  kubectl get pods
  ```

- **View Services**:
  ```bash
  kubectl get services
  ```

## 🚀 Getting Started with Kubernetes

1. **Create a Deployment YAML File**:
   A Deployment YAML file defines a set of Pods and the desired state of the application. Here’s an example:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-app
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: my-app
     template:
       metadata:
         labels:
           app: my-app
       spec:
         containers:
           - name: my-app-container
             image: nginx
             ports:
               - containerPort: 80
   ```

2. **Apply the Deployment**:

   ```bash
   kubectl apply -f deployment.yaml
   ```

3. **Scale the Deployment**:

   ```bash
   kubectl scale deployment my-app --replicas=5
   ```

4. **Delete the Deployment**:
   ```bash
   kubectl delete deployment my-app
   ```

## 📚 Further Reading

- [Kubernetes Official Documentation](https://kubernetes.io/docs/home/)
- [Kubernetes Tutorials](https://kubernetes.io/docs/tutorials/)
- [Kubernetes CLI Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

---
