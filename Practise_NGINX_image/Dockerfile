#This Dockerfile installs NGINX on Ubuntu and creates a non-root application user. 
#The container runs NGINX in the foreground so Docker can monitor its main process and 
#manage the container lifecycle

# Uses Ubuntu as the base image
FROM ubuntu:24.04

# Prevents interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Updates package lists, installs NGINX, and removes package lists
# afterward to reduce unnecessary image contents
RUN apt-get update \
    && apt-get install -y --no-install-recommends nginx \
    && rm -rf /var/lib/apt/lists/*

# Creates a non-root user with UID 1000
RUN useradd -m -u 1000 appuser

# Sets the default working directory
WORKDIR /app

# Documents that NGINX serves traffic on port 80
EXPOSE 80

# Runs NGINX in the foreground so Docker can manage the process
CMD ["nginx", "-g", "daemon off;"]