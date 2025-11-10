# Kubernetes Deployment Explanation

This document explains the design and implementation decisions made during the deployment of the Week 5 Independent Project on Google Kubernetes Engine (GKE). It aligns with the rubric and demonstrates reasoning for all implementation choices.

# 1. Choice of Kubernetes Objects
# a. Deployments

Frontend Deployment: Manages web UI pods.

Backend Deployment: Manages API/service logic pods.

Reasoning: These components are stateless, allowing easy scaling and automatic replacement of failed pods. Deployments ensure that the desired number of replicas is maintained at all times, providing high availability and fault tolerance.

# b. StatefulSet

MongoDB StatefulSet: Deploys the database.

Reasoning: Databases require stable network identities and persistent storage. StatefulSets ensure each pod receives:

# A unique hostname.

A dedicated PersistentVolumeClaim (PVC) for persistent data storage.

Benefit: The database maintains integrity even if pods are terminated, rescheduled, or scaled down.

# c. Services

Frontend Service (LoadBalancer): Exposes the frontend to the internet and automatically provisions a public IP in GKE.

Backend Service (ClusterIP): Provides internal communication between backend pods and the frontend.

MongoDB Service (Headless): Provides stable DNS hostnames for StatefulSet pods, allowing backend pods to reliably connect to the database.

# 2. Method to Expose Pods to Internet Traffic

The frontend is accessible externally via a LoadBalancer and public IP:
http://<EXTERNAL-IP>:80

Backend and MongoDB are exposed internally using ClusterIP and Headless Service, respectively, ensuring security and controlled communication.

# 3. Persistent Storage

MongoDB pods use PersistentVolumeClaims mounted at /data/db.

Data Persistence: PVCs retain data even if the StatefulSet is scaled down or deleted, ensuring database consistency.

Verification: Data remains intact across pod restarts; backend continues to access the same data without loss.

# 4. Git Workflow and Version Control

A structured workflow was maintained, documenting incremental progress:

Commit	Description
Updated backend and StatefulSet	Adjusted backend deployment and MongoDB StatefulSet
Updated mongo-statefulset.yaml	Fixed volume configuration and storage class
Created and added MongoDB StatefulSet	Added initial StatefulSet for MongoDB with PVC
Added backend deployment	Added backend Deployment and service manifests
frontend deployment	Added frontend Deployment and service manifests
Added mongo-service.yaml	Created MongoDB headless service manifest
Added  backend service file	Created ClusterIP service for backend
Created manifests directory and frontend-service.yaml	Organized files and created frontend service

Benefit: Demonstrates incremental progress, proper version control, and clear documentation.

# 5. Application Functionality

Frontend → Backend: Frontend communicates via the internal ClusterIP service.

Backend → MongoDB: Backend connects to the MongoDB StatefulSet using DNS:
mongodb://nate:<root_password>@mongo-0.mongo.yolomy.svc.cluster.local:27017/yolomy?authSource=admin

Data Persistence: Items added through the frontend persist in the database, demonstrating effective use of Persistent Volumes.

# 6. Resource Management

Requests and Limits: CPU and memory requests and limits are defined for all pods to ensure stable operation, especially in GKE Autopilot.

QoS Class: Pods with requests and limits fall under Burstable, balancing guaranteed resources with flexibility.

High Availability: Backend and frontend have 2 replicas each; MongoDB StatefulSet ensures database reliability.

# 7. Best Practices Implemented

Tagged Docker Images: Ensures traceability and reproducibility.

Namespace Isolation: All resources are deployed in the yolomy namespace to isolate this project from other cluster workloads.

Labels and Annotations: Used consistently for easy management, monitoring, and scaling.

Separation of Concerns: Backend, frontend, and database are deployed as separate Kubernetes objects.

StatefulSet for Database: Provides stable storage and unique pod identities.

# 8. Conclusion

This project demonstrates a fully functional, persistent, and scalable Kubernetes application on GKE.
The combination of Deployments, StatefulSets, PersistentVolumes, and properly defined Services ensures:

Application reliability

Data durability

Internal and external networking

Best practices adherence for production-ready deployments
