# Build Stage
FROM node:14-alpine AS build

# Working Directory
WORKDIR /usr/src/app

# Copy Package files
COPY package*.json ./

# Install Dependancies
RUN npm install --no-package-lock

# Copy the Source
COPY . .

# Multi-stage build
FROM alpine:3.18 AS prod

WORKDIR /usr/src/app

RUN apk update && apk add --update nodejs

COPY --from=build /usr/src/app ./

# The port the app will run on
EXPOSE 5000:5000

# Comand that will run the app
CMD ["node", "server.js"]
