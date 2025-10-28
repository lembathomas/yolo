# Thomas Yolo E-commerce Platform - Playbook Explanation

## Overview

This document explains the structure and execution order of the Ansible playbook used to deploy the e-commerce platform on a Vagrant-provisioned Ubuntu VM. The playbook is organized into **roles**, each handling one microservice (MongoDB, Backend, Client), and uses **variables** for flexibility and maintainability.

---

## Playbook Execution Flow

The main playbook (`playbook.yaml`) executes sequentially:

1. **Mongo Role**  
   - **Purpose:** Set up the database before any dependent services (backend/client) start.  
   - **Tasks:**  
     1. Install Docker if not already present (`ansible.builtin.package`).  
     2. Pull the MongoDB image from Docker Hub (`community.docker.docker_image`).  
     3. Run the MongoDB container with environment variables for root username and password, and attach a persistent volume (`community.docker.docker_container`).  

   **Rationale:** MongoDB must be running first because the backend depends on it to connect to the database.  

2. **Backend Role**  
   - **Purpose:** Deploy the Node.js API server that interacts with MongoDB.  
   - **Tasks:**  
     1. Pull the backend Docker image from Docker Hub.  
     2. Run the backend container, exposing port 5000.  
     3. Wait for the backend service to be up and ready (`ansible.builtin.wait_for`).  

   **Rationale:** Backend is deployed after MongoDB to ensure it can establish a database connection on startup. Waiting for the service prevents timing issues for the frontend role.  

3. **Client Role**  
   - **Purpose:** Deploy the frontend web application.  
   - **Tasks:**  
     1. Pull the client Docker image from Docker Hub.  
     2. Run the client container, exposing port 3000.  
     3. Wait for the client service to be up and ready (`ansible.builtin.wait_for`).  

   **Rationale:** The client is dependent on the backend API, so it is executed last. Waiting ensures the frontend can connect to a fully operational backend.  

---

## Use of Variables

All key configuration parameters are stored in `vars/main.yaml`, including:

- Docker image names for MongoDB, Backend, and Client.  
- Container names.  
- Port mappings.  
- MongoDB root credentials.  

**Benefits:**  

- Avoids hardcoding values in roles.  
- Makes the playbook modular and easy to adapt to different environments.  
- Simplifies role reuse and maintenance.  

---

## Task Design Principles

1. **Modular Roles:** Each service has its own role to isolate responsibilities.  
2. **Idempotence:** Tasks are designed to be safe to rerun without causing duplication or errors.  
3. **Sequential Execution:** Roles are ordered based on dependencies (Mongo → Backend → Client).  
4. **Permission Management:** All Docker tasks use `become: true` to prevent permission errors when interacting with the Docker daemon.  
5. **Waits for Services:** `wait_for` ensures services are fully operational before dependent roles start.  

---

## Testing and Verification

- SSH into the VM (`vagrant ssh`) to check container status:

```bash
docker ps
Test backend API: http://localhost:5000

Test frontend client: http://localhost:3000

Add a product through the frontend form to confirm MongoDB persistence.

Conclusion
This playbook demonstrates best practices for Ansible-driven container orchestration:

Proper role separation.

Use of variables for flexibility.

Sequential, dependency-aware execution.

Robust deployment with waits and permissions handled correctly.

The system is ready to run on any fresh Ubuntu VM provisioned with Vagrant.