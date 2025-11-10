# Moringa IP – Kubernetes Deployment

This project deploys a full-stack application consisting of a frontend, backend, and MongoDB database on Kubernetes.
 The setup demonstrates containerized microservices running in a scalable and resilient cluster environment.

# Project Structurec
```
├── Explanation.md
├── README.md
├── manifests/
│ ├── backend-deployment.yaml
│ ├── backend-service.yaml
│ ├── frontend-deployment.yaml
│ ├── frontend-service.yaml
│ ├── mongo-statefulset.yaml
│ ├── mongo-service.yaml
│ └── namespace.yaml
```


Each manifest defines the Kubernetes resources required to deploy and expose the application components within the cluster.

# Components Overview
1. Frontend

Image: lembadev/frontend:latest
Deployment Type: Deployment
Replicas: 2
Port: 3000
Service Type: LoadBalancer
The frontend connects to the backend through the internal service name defined in the Kubernetes cluster.

2. Backend

Image: lembadev/backend:latest
Deployment Type: Deployment
Replicas: 2
Port: 5000
Service Type: ClusterIP
The backend communicates with MongoDB using the internal DNS of the StatefulSet.

3. MongoDB

Image: mongo:6.0
Deployment Type: StatefulSet
Replicas: 1
Storage: 1Gi PersistentVolumeClaim

# Environment Variables:

MONGOINITDBROOTUSERNAME: username
MONGOINITDBROOT PASSWORD: passwoed

Service Type: Headless (ClusterIP: None)
The MongoDB StatefulSet provides persistent storage and stable network identity for the database pod.

# Networking and Access

Frontend Access:
The frontend service is exposed as a LoadBalancer. When deployed on Minikube, use:
minikube service frontend-app
or if using an external environment, access via the assigned external IP.

# Backend Service:
The backend runs as an internal ClusterIP service and communicates directly with the frontend and MongoDB.

# Deployment Steps

Apply the MongoDB StatefulSet and Service
kubectl apply -f manifests/mongo-service.yaml
kubectl apply -f manifests/mongo-statefulset.yaml
Apply the Backend Deployment and Service
kubectl apply -f manifests/backend-deployment.yaml
kubectl apply -f manifests/backend-service.yaml
Apply the Frontend Deployment and Service
kubectl apply -f manifests/frontend-deployment.yaml
kubectl apply -f manifests/frontend-service.yaml


# Verify all Pods
kubectl get pods -n yolomy


# Access the Application
kubectl get svc -n yolomy

# Commit History Summary
Commit Message	
Updated backend and StatefulSet	
Updated mongo-statefulset.yaml	
Created and added mongo StatefulSet	lembathomas	Nov 9, 2025
Added backend deployment	
Added frontend deployment	
Added mongo-service.yaml	
Added backend service file	
Created manifests directory and frontend-service.yaml	

# Namespace
All components are deployed under the yolomy namespace for better resource organization.

# Persistence and Data Management

The MongoDB StatefulSet uses PersistentVolumeClaims (PVCs) to ensure data durability. 
The data persists even when pods are rescheduled or replaced.

# Scaling
The frontend and backend deployments can be scaled easily:
kubectl scale deployment frontend-deployment --replicas=3 -n yolomy
kubectl scale deployment backend-deployment --replicas=3 -n yolomy


MongoDB remains a single replica for consistency, but can be extended for high availability using replica sets.

# Conclusion

The Kubernetes setup successfully deploys a full-stack application where the frontend, backend, and MongoDB communicate seamlessly within the cluster.
It demonstrates container orchestration, persistent storage, and internal/external networking under a single namespace.
