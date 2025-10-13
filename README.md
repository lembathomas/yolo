

Overview

This project is a containerized full-stack e-commerce application built with React (frontend), Node.js/Express (backend), and MongoDB (database).
The application allows users to view and add products through a dashboard interface. The setup demonstrates the use of Docker, Dockerfiles, and Docker Compose for multi-container orchestration, networking, and data persistence.

How to Build and Run

Prerequisites

Docker and Docker Compose installed.
Git installed.

Steps

Clone this repository:

git clone https://github.com/<your-username>/<your-repo>.git

cd <your-repo>


Build and run the containers:

docker-compose up --build

To stop containers:
docker-compose down


To remove all containers, networks, and volumes:
docker-compose down -v