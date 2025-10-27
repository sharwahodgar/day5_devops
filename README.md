# 🐳 Local Kubernetes Deployment using Minikube and Docker

This repository contains the configuration files and a technical guide for setting up and managing a basic web application (Nginx) within a local Kubernetes cluster using **Minikube**.

## Project Objective

The goal of this project was to understand the fundamental concepts of Kubernetes deployments, services, and scaling by simulating a cluster environment locally.

---

## 1. Architecture Overview (The Components)

| **Component** | **Description** | **Role in the Project** |
| :--- | :--- | :--- |
| **Minikube** | A tool that runs a single-node Kubernetes cluster inside a lightweight Virtual Machine (VM) or a Docker container. | The local simulation of a Kubernetes Master and Worker Node. |
| **Docker** | Containerization platform used by Minikube as the virtual driver. | Hosts the Minikube VM and the application containers (Nginx). |
| **`kubectl`** | The command-line interface (CLI) used to communicate with the Kubernetes cluster. | The "Remote Control" for deploying and managing resources. |
| **Deployment** | A controller that defines the desired state (e.g., image, number of replicas). | Defined in `deployment.yaml`. Ensures 5 Nginx Pods are running. |
| **Pod** | The smallest deployable unit in Kubernetes, hosting one or more containers. | Each Pod runs a single `nginx:latest` web server container. |
| **Service** | An abstraction that defines a logical set of Pods and a policy for accessing them. | Defined in `service.yaml`. Provides a fixed IP/Port (`NodePort`) to access the Nginx Pods. |

---

## 2. Setup and Installation Guide

These steps assume you are running **Windows 11 Pro** (as identified during the setup process) and have Docker Desktop installed and running.

### A. Environment Preparation

1. **Install Tools:** Ensure you have `minikube` and `kubectl` installed. (For Windows 11, use `winget install Kubernetes.kubectl`).

2. **Start the Cluster:** Launch the Minikube cluster using the Docker driver:

   ```bash
   minikube start --driver=docker