YOLO E-COMMERCE-APP Explanation 
This appication includes a backend, front and database 

BASE IMAGE CHOICE 


BACKEND (Node.js API)
The choice here was node:14-alpine since it is very compatible with Node.js applications 
and using it ensure much quicker builds and small final images

FRONT-END (CLIENT)
The choice was node:14-alpine for build stage, and nginx:alpine for production stage.
The multi-stage build ensures only static build files are served in production, reducing the final image are small

MongoDB - Database Image 
The choice for this image was mongo:6.0 an official MongoDb image that is stable 
and includes all the dependancies and is optimized for container use

Dockerfile Directives and Purpose for Each Microservice

Backend Dockerfile 
FROM node:14-alpine
Lightweight Node environment for installing dependencies and building the app.

WORKDIR /usr/src/app 
Defines the working directory inside the container.

COPY & RUN npm install 
Installs dependencies from package.json.

COPY . . 
Copies source files to the container.

FROM alpine:3.18 
Creates a minimal runtime environment for the final build.

EXPOSE 5000
Makes the backend accessible via port 5000.

CMD → Defines the command to start the app.


Backend Dockerfile 
This Dockerfile uses a multi-stage build to efficiently create and serve a React app. 
Stage 1
The first stage uses the lightweight node:14-alpine image to install dependencies and build optimized production files with npm run build. 
Stage 2
The second stage uses nginx:alpine to serve the static files, removing the default Nginx content and copying the React build into its web directory. 
It then exposes port 80 to make the application accessible over HTTP.

Database
Pulls mongoDB image that will be used to create a custome database image.

Docker-Compose Networking
Created to networks to help in connecting the 3 microservice backend_net and lemba_net
backend_net connects backend to databaase and lemba_net connects frontent(client) to the backend
Both network use the bridge driver and are created using docker-compose.yaml file
This separation improves security by isolating services that do not need direct communication.


Port mapping:
Frontend → 3000:80
Backend → 5000:5000
MongoDB → 8000:27017


Docker Volumes (Persistence)
MongoDB’s data is stored persistently using a named volume: mongo_data:/data/db and declared globally as ongo_data
This is important as it ensures that product data (and other database entries) are retained even if the Mongo container is rebuilt or restarted

Git Workflow
Added Dockerfiles and docker-compose.yml step-by-step with descriptive commits.
Committed after each successful test, ensuring progress tracking.
Final push includes: explanation.md, Updated README.md, Screenshot of DockerHub images.

Screenahots of images pushed to Dockerhub
Client-docker-screenshot.png
backend-docker-screenshot.png
