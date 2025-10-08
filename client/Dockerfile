# Build Stage
FROM node:18-alpine AS build

# Working Directory
WORKDIR /usr/src/app

# Copy Package or dependency files
COPY package*.json ./

# Install Dependancies
RUN npm ci

# Copy the Source code
COPY . .

# Build the production for client 
RUN npm run build

#Production Stage

FROM nginx:alpine AS prod

#Remove Nginx static files
RUN rm -rf /usr/share/nginx/html/*

# Copy build output from previous Stage
COPY --from=build /usr/src/app/dist /usr/share/nginx/html

WORKDIR /usr/src/app

# The port the app will run on
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]