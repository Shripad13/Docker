#  Example #1 — Node.js App with Nginx

# “In one project, I used multi-stage Docker builds to separate the Node.js build environment from the production runtime. 
# The first stage used a Node image to install dependencies and run the build, while the second stage used a lightweight Nginx image to serve the static files. 
# This reduced our image size from ~900MB to under 50MB, improved security, and sped up deployments in Kubernetes.”

#I use a multi-stage build for the frontend. Node.js installs dependencies and generates 
#production-ready static files. Then I copy only the build output into a lightweight NGINX image. 
#This avoids shipping Node.js, development dependencies, and source code unnecessarily in the 
#production image."

# Stage 1: Build the frontend application
FROM node:18 AS builder
# Uses Node.js 18 to install dependencies and build the frontend

WORKDIR /app
# Sets /app as the working directory

COPY package*.json ./
# Copies package.json and package-lock.json, if present
# This improves Docker layer caching when source files change

RUN npm ci
# Installs dependencies from the lock file for reproducible builds
# Requires a compatible package-lock.json

COPY . .
# Copies the application source code into the builder stage

RUN npm run build
# Executes the build script defined in package.json
# Typically generates static files in the dist directory

# Stage 2: Serve the static frontend using NGINX
FROM nginx:alpine
# Uses a lightweight NGINX image as the production web server

COPY --from=builder /app/dist /usr/share/nginx/html
# Copies the generated frontend files into NGINX's default web root

EXPOSE 80
# Documents the HTTP port

CMD ["nginx", "-g", "daemon off;"]
# Starts NGINX in the foreground