# Thomas Yolo E-commerce Platform

## Project Overview

This project automates the deployment of a containerized e-commerce web application using **Ansible** and **Docker** on a **Vagrant-provisioned Ubuntu VM**. The system consists of three microservices:

1. **MongoDB** – Database service.
2. **Backend** – Node.js API server.
3. **Client** – Frontend web application.

The goal is to automate provisioning, container deployment, and service orchestration while following best practices including **roles, variables, and modular playbooks**.

---

## Project Structure

playbook/
├── playbook.yaml # Main Ansible playbook
├── inventory # Hosts inventory
├── vars/
│ └── main.yaml # Global variables
├── roles/
│ ├── mongo/
│ │ └── tasks/main.yaml
│ ├── backend/
│ │ └── tasks/main.yaml
│ └── client/
│ └── tasks/main.yaml
└── Vagrantfile # Vagrant configuration for Ubuntu VM


## Prerequisites

- **Vagrant** installed on the host machine.
- **VirtualBox** as the provider for Vagrant.
- Internet connection for pulling Docker images from Docker Hub.
- **Ansible** installed on the host machine.

---

## Setup Instructions

1. **Clone the repository**:

git clone <repository-url>
cd <repository-folder>
Start the Vagrant VM:

bash
Copy code
vagrant up
This provisions the VM and runs the Ansible playbook automatically.

Verify containers:

SSH into the VM:

bash
Copy code
vagrant ssh
docker ps
You should see:

MongoDB container

Backend container

Client container

Access the application:

Backend API: http://localhost:5000

Frontend client: http://localhost:3000

Variables
All service configuration is stored in vars/main.yaml:

Docker image names

Container names

Ports

Environment variables for MongoDB

This allows easy modification without editing roles or playbooks.

Roles
Mongo
Installs Docker if missing.

Pulls MongoDB image from Docker Hub.

Starts MongoDB container with persistent data volume.

Backend
Pulls backend Node.js image from Docker Hub.

Starts the backend container.

Waits for the backend API to be ready.

Client
Pulls frontend image from Docker Hub.

Starts the client container.

Waits for the client application to be ready.

Notes
All Docker tasks use become: true to avoid permission issues.

Roles are modular and can be run independently.

MongoDB volume ensures persistent storage for testing added products.
