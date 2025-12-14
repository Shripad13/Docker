#  Example #1 — Node.js App with Nginx

# “In one project, I used multi-stage Docker builds to separate the Node.js build environment from the production runtime. 
# The first stage used a Node image to install dependencies and run the build, while the second stage used a lightweight Nginx image to serve the static files. 
# This reduced our image size from ~900MB to under 50MB, improved security, and sped up deployments in Kubernetes.”


# Stage 1 - Build
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install --production=false
COPY . .
RUN npm run build

# Stage 2 - Nginx runtime
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
