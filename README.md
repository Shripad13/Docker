# Docker

Phrase  "Works on my machine?"
This is the problem Docker kills.

Container - A running box that holds your app & its dependencies.
Image - Recipe of container.
Dockerfile - Plain text file with instructions to build the image.

1. docker build -t myapp .
2. docker run -p 3000:3000 myapp
3. docker ps
4. docker stop <containerID>
5. docker compose up  ---> starts ur whole stack in 1 command - App+DB+cache all together

docker-compose.yml in this you can define APP+DB+RedisCache
then type docker compose up and everything starts together, connected working.

##


A one physical machine is capital intensive 

Hypervisor is a software of virtualization.
ESXI is a software from VMware for virtualization.
Hyper-v is a software from Microsoft for virtualization.


# What is Bare-Metal?
In DevOps, bare-metal refers to running software directly on physical hardware without a virtualization layer (like a hypervisor, VM, or container abstraction in between).
Bare-metal = physical server + operating system installed directly on it

# What is Virtual Machine?
A virtual machine (VM) is a software emulation of a physical computer that runs an operating system and applications just like a physical machine. It allows multiple VMs to run on a single physical host, each with its own isolated environment, including its own OS, CPU, memory, and storage. VMs are created and managed using hypervisor software, which allocates resources from the physical host to each VM. This technology enables efficient use of hardware resources, flexibility in deployment, and isolation between different workloads.

Setup                        Description
Bare-metal              App runs directly on physical hardware
Virtual machine (VM)    App runs inside a virtual environment created by hypervisor
Containers (Docker/K8s) App runs in lightweight isolated environments sharing OS

Bare-metal setup --> Hardware → OS → Application
VM setup         --> Hardware → Hypervisor → VM → OS → Application
Container setup  --> Hardware → OS → Container runtime → Containers → Application

# Why containers are light weight than VMs?
1. In VMs, each VM has its own OS, whereas in containers, all containers share the host OS kernel.
2. VMs require more resources (CPU, memory, storage) to run multiple OS instances, while containers are more efficient as they run isolated applications on a single OS.
3. Containers start up faster than VMs because they don't need to boot an entire OS.
4. Containers use less disk space since they share common OS layers, while VMs require separate OS installations.
5. Containers are more portable and easier to deploy across different environments compared to VMs.
6. Containers provide better resource utilization and scalability compared to VMs.

# Container -
A container means a package that has needed things in place & shipping it.
1. Container is a package with just application, application libraries & the OS Modules.
2. If a container is working on my machines, it should also work on the other machines.
3. With containers you dont spend too much on the hardware.
4. With containers, you can start the container in leass than 1 sec & can stop the container in sec

Serverless means your not responsible for managing the servers. Cloud provider is the responsible for managing the server.

## How to run the Containers?
1. you should have a linux machine (flavour can be any opensource linux)
2. Container runtime should be installed on that machine.
3. Just run the Containers on the top of it.

# [Misconception - Docker means container & container means docker]

Docker is just a container runtime (like provides environment or platform to run the docker images) & in the space of containers it was highly used.


# NameSpaces - 
NameSpaces are meant to isolate the resources(applications).
(beacause if 2-3 appln runs on same docker then NameSpace will isolate the appln by network)
For ex - netns is a network namespaces which isolates the network for containers.

# Control Groups (C Groups) -
Containers capable of consuming all the resources of OS, yet we need to control or limit the resources and that part will be done by Control Groups.


# Podman is also a Container runtime from RedHat based software.

## WHy Docker became quite famous?
1. Docker has built a very good & easy to use eco-system.
2. Very good UX (User Experience)
3. Supports a very HIGH level features.

Docker-CE (Actual docker, use by companies)
Docker (it has podman which is offered by Redhat)

############################################################################################

> Dockerfile ---Build--->>  Docker Image ----Run--->> Docker Container

## Containerization - 
1. Build own containers using the base images from docker hub.
2. lets run them on the top of our servers.
 
A process to make your own images by taking the references as base images is called as containerization.

# To make our images, we need to understand how it works?

> Dockerfile (This is the file where we are going to place all the instructions & this works in a Instruction Argument approach)
> Dockerfile is the only file name for Docker containers, there is no extension for the file.

> dockerfile commands -  https://docs.docker.com/reference/dockerfile/

# What are the best practices of Docker Imaging?
1. Size of the Docker Image has to be as minimal as possible.
2. Security of the Images, It should be zero to none vulnerabilities as per your organization.
3. Always ensure to install what is really needed, to keep image simple & secure.
4. Ensure containers dont run as a root user

# Why Docker containers should not be run with root user in linux machines

I follow the principle of least privilege and avoid running application containers as root. 
A root process inside a container has unnecessary privileges, and if the application is compromised, those privileges increase the potential impact. 
I create a dedicated non-root UID/GID in the Dockerfile and use USER, or enforce runAsUser at the Kubernetes level. 
I also combine this with dropping unnecessary Linux capabilities, read-only root filesystems, seccomp/AppArmor, resource limits, and avoiding privileged containers or Docker socket mounts.

EX - 
---
FROM ubuntu

RUN apt-get update && apt-get install -y nginx

RUN useradd -m -u 1000 appuser

USER appuser

CMD ["nginx", "-g", "daemon off;"]

---

## AMI
Base AMI -------> Configure the OS, Install the needed packages -----> AMI

## Docker Image
Base Image -------> Customize the Image -------> Publish the Image

# Preferred Pattern for containerization - (bcoz we dont need to maintain nginx here)
1. Take nginx image from base
2. Do the customization
3. Publish it.

# No Preferrable way for containerization-
1. Take centos as a base image
2. Install nginx
3. Customize it 
4. Publish it

## Container Registries - 
Docker hub - docker.io
AWS ECR - ecr.io
GCP GCR  - gcr.io


# Container Naming Standards - 

Ex - docker.io/userName/containerName:tag

docker.io - repo name
userName - Account name
containerName- name of Container
tag - Version


$ docker pull imageName:version
$ docker run containerImage

if version is not mentioned then bydefault it will pull latest image

## Containers are immutable -
There is no concept of start or stop.
Just run , means container will be created, task will be executed & container will be killed.
You cannot make changes on a container, even if you make you would lose them & you have to make the changes on the image.

Immutable (Unchangeable) which cannot be changed
Mutable which can be changed.

Containers are not like OS, they are meant to run a single process at a time.

Stopping the container means killing the container
When you START the container means it will create a brand new Container

> Our underline server will run on different network
> Also Containers run on different network

> Overlay Network enables the communication between server network & container network.

# Images Directory
Docker images directory - /var/lib/docker
Podman images directory - /var/lib/containers/storage

# you can see containers on Standard USer -  $HOME/.local/share/containers/storage/overlay

# what is bridge network in docker?
A bridge network in Docker is the default internal network that Docker creates for containers on a single host.
A Docker bridge network is an internal network that allows containers on the same host to communicate.
Each container gets its own IP address within the bridge subnet.
Containers can also access the host network via NAT, but by default are isolated from external networks unless ports are exposed.

1. Default network: If you don’t specify a network, Docker attaches containers to bridge.
2. Container-to-container communication: Containers on the same bridge can talk using IP or container name.
3. Port mapping to host: To access a container externally, you map container ports to host ports (e.g., -p 8080:80).
4. Custom bridge networks: You can create your own bridge network for better DNS resolution and isolation.

# Example- 
docker network create my-bridge
docker run --network my-bridge --name web nginx
docker run --network my-bridge --name app my-app
Here, web and app can communicate using container names (web, app).
\\
docker network ls
docker network rm -f my-bridge
docker network  create my-docker-bridge
docker run -d --name nginx-network-test --network my-docker-bridge nginx:latest
docker network inspect  nginx-network-test|grep "NetworkMode"
\\****

############################################################################################

## what is the command to install docker edition on linux server rhel 9

# 1. Remove old Docker versions (if any)
sudo dnf remove docker docker-client docker-client-latest docker-common docker-latest \
docker-latest-logrotate docker-logrotate docker-engine -y

# 2. Install required packages
sudo dnf -y install dnf-plugins-core

# 3. Add the Docker repository
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo

# 4. Install Docker Engine and containerd
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 5. Enable and start Docker
sudo systemctl enable --now docker

# 6. Verify installation
sudo docker run hello-world


# To install docker as podman

sudo dnf install docker -y

# Docker Commands
docker ps           ---> To show list of running containers
docker images        ---> To show list of available  containers
docker run nginx:latest
docker rm nginx:latest   ----> it will run on terminal
docker run -d nginx:latest   ----> it will NOT run on terminal else run in detached mode
d - means detached mode


sudo docker inspect <container ID>   ----> it will show properties of container, each container will have seprate IP address
sudo docker stop <conrainer ID>

docker ps -a    ----> shows a containers which have ended 

___________________________________________________________

# To remove all Docker images - 

> docker rmi -f $(docker image ls -a -q)

quiet mode (-q), meaning it prints only their image IDs
Lists all Docker images (including intermediate ones -a),
Command substitution $( ... ): runs the inner command and passes its output as arguments to the outer command.
Force-removes (-f) the listed Docker images by ID


___________________________________________________________

## Docker Port

sudo docker run -d -P nginx:latest
-P means Randomly allocates the port


sudo docker run -d -P 80:80 nginx:latest
--docker process runs on 80 redirect to port 80 for internet

Command to enter into container
sudo docker exec -it <container id>  --sh/--bash

--sh /--bash - enters into shell/bash terminal

___________________________________________________________

docker logs -f <Container ID>        ---> To check the logs , -f stream the logs

docker run -d -e MYSQL_ROOT_PASSWORD=password8 mysql

-e - passing a env variable

docker exec it <container id>  -sh

mysql -uroot -ppassword8

show databases;
___________________________________________________________
## COntainerization/Write Docker file & build image

docker build .
docker images
docker ps
docker run -d -p 81:80 <container id>

docker ps
docker inspect <container id>

Use Docker Hub or Amazon ECR for image storage

# How do you monitor and manage logs of running containers?
1. Logging & Monitoring: Centralized logging solutions like ELK Stack
(Elasticsearch, Logstash, Kibana).
2. VIew logs - docker logs -f container_name
3. Configure log driver in docker-compose.yml

# difference between CMD & ENTRYPOINT in docker?

CMD & ENTRYPOINT are a kind of startup for container

Values in ENTRYPOINT cannot be overriden during the runtime
Values in CMD can be Overriden during the runtime

1. you can have n number of CMD, but only the latest will be considered in the Dockerfile
2. CMD is typically used to pass the arguments (that means values mentioned in CMD can be overriden)
3. Values in ENTRYPOINT cannot be overriden

When you want to use CMD & ENTRYPOINT together?
ENTRYPOINT ["ping] --> It will not overridden
CMD ["google.com]  --> It can be overridden during runtime

lets say if we want to change google.com to flipkart.com then you can run flipkart.com in runtime.

Ex - 
 $ ls -ltr
ls means ENTRYPOINT, -ltr means CMD

In docker, if you mention any process by in a JSON format, it will be process of its own where the parent process id directly 1 which is the system process.


ENTRYPOINT ["java", "-jar", "app.jar"]
means that whenever a container starts from this image, Docker will execute:
java -jar app.jar
Doesn't invoke a shell.
Avoids shell parsing issues.

## Docker containers stores at - 
/var/lib/containers/storage/overlay-containers/

## How to publish the Docker images to docker hub?

 $ docker login docker.io
--> enter your username & pwd for authentication
 $ docker tag simplecalculaor-multistage:latest batminton98/simplecalculaor-multistage:v1
 $ docker push batminton98/simplecalculaor-multistage:v1
 $ docker tag mysql:8.0 batminton98/mysql:8.0-v1
 $ docker push batminton98/mysql:8.0-v1
 $ docker push docker.io/sanraman/expense-base/frontend:v1

___________________________________________________________

## To Increase the Space or Disk 

Run throguh root user - 

df -kh 
lsblk     ----> you can see Total disk size at 1st line itself,  Also check each mount point disk space.
sudo vgs  ----> to check virtual FREE Root Volume size
sudo fdisk -l       ----> To check each device start & end size 

1. Add the disk through AWS ELB.

2. Expand the disk
sudo growpart /dev/xvda 4   (4 is the partition number)

3. Expand the Logical Volume of MountPoint

sudo lvextend -l +50%FREE /dev/mapper/RootVG-homeVol
sudo lvextend -l +100%FREE /dev/mapper/RootVG-homeVol
sudo lvextend -r -L +6G  /dev/mapper/RootVG-homeVol

4. Expand the FileSystem
sudo xfs_growfs /var
sudo xfs_growfs /home

5. Check with
df -Tkh


######## Docker Volumes Mapping ####
1. Containers are ephermal & hence we lose the data if the container is deleted.
2. ephermal - The container is temporary and short-lived – all its data and state are lost when it stops or is removed
3. In cases, we need to have the data persistent even if we lost or remove the container.
4. A volume of host machine can be mapped to a container using -v option.
5. Filesystem is temporary – any changes inside the container are not saved to the host.
6 .Stateless design – containers should store persistent data in volumes or external storage.
7. Easy to recreate – you can stop, delete, and redeploy the container without losing the app setup.
   

-v <hostPath>:<containerPath>


##### StateFul Vs Stateless #########

Apps are of 2 types - Stateful & Stateless 

Apps which are dependent on storage or data are called as stateful appln
Apps which are NOT dependent on storage or data are called as stateless appln

# Docker containers are inherently stateless.
By default, any changes made inside a container (files, logs, databases) are lost when the container stops or is removed.
To make a container stateful, you need to use volumes, bind mounts, or external storage to persist data.

Ex- Database is stateful application.
Frontend and Backend are stateless application because even if you restart you will not lose data.

> Containers are stateless.  

## Drawbacks of Docker - 

1. What will happen if you lose any of the server (assuming containers are running on 4-5 server)?
what happens to the running containers?
--> we will lose containers & data

2. If you have 100 servers with container runtime, how do you manage them? how can you login to them?
3. If one of your run time has less containers & others have more containers & higher utlilization?
who is going to move in between?

4. Because of any reason, if you lose the container, someone has to come & start it ? Agreed?

Like in Google, they have millions of Containers running so
Running containers on Instances is not reality
To solve all this problems CONTAINER ORCHESTRATION will come into picture.

KUBERNETES is top-notch Container Orchestration Tool.


###################################################################################

### Docker supports several commands we can use to build images. Some common ones:

FROM: Initializes a new build stage and sets the base image for subsequent instructions.
WORKDIR: The working directory for the container.
COPY: Copies files from location on the host to the image.
ADD: Copies files from a location on the host to the image. Add also enables copying from a URL and extracting the contents of a tar file to the image.
RUN: Executes a command and saves the results as a new layer in the image.
MAINTAINER: The author or maintainer of the image. [Deprecated]
LABEL: A key-value pair to store metadata about the container.
BUILD: Defines a variable to pass to the build command.
FOREGROUND - A process run as Active process of container


> Dockerfile ---Build--->>  Docker Image ----Run--->> Docker Container


# Steps -
1. Through Terraform Create Instance
2. Through Ansible Deploy the docker


> No one runs the container on EC2 instance, instead they uses Container Orchestration tools.


## Challenges with Running Containers on VM's?
 1. We are responsible for managing & manitaining the underlying infrastructure.
 2. We are responsible for manitaining the container runtime.
 3. NETWORK is not tightly coupled, what if you have more than 3 containers of frontend, 3 containers of backend, how can you communicate? 
 4. If containers are running on independent machines, how to achieve the shared STORAGE concept?

Main problems are Common Network & Shared Storage

 > TO overcome above challenges, we can use K8s Container Orchestration Tool.



 > Amazon Prime moved from microservices to monolith - its a prime example of moving containers to VM's
It depends on what your application & business required.



## multi-stage build
A multi-stage build in Docker is a technique that allows you to use multiple FROM statements in a single Dockerfile—each one creating a separate build stage. This helps you build software in one stage and then copy only the necessary artifacts into a smaller, cleaner final image.

In Multi stage Build Docker file will have n number of stages but final stage will have minimal size of image

# Why it’s useful ?
Produces much smaller images
Keeps build tools and dependencies out of the final image
Improves security by reducing attack surface
Makes builds more efficient and organized

## How it works ?
The build stage installs dependencies and compiles the code.
The final stage starts clean (nginx), copies only the compiled output, and excludes all build-time dependencies.

# Distroless images
Distroless images are minimal container base images that contain only your application and its runtime dependencies, completely stripped of a standard Linux operating system distribution

what is the one of the issue with docker containers and how did you solve it?
Previously we were using Ubunt base images and Java/Python runtime images which were exposed to some kind of vulnerability by hackers
Found some issues with existing images, and we moved to python distroless images which have only python runtime , also it does not have some basic packages like curl, zip, find, so it was providing the hisghest level of security, after implementing the distroless images we are safe to say that pur application not exposed to OS or application vulnerabilities.

with Distroless images we are not only reducing the sixze of imgae but also containers runnign securely

# what is Mutable & immutable Infrastructure?
Immutable Infrastructure is a concept where once a server or component is deployed, it is never modified. If an update or change is needed, a new version of the server or component is built and deployed, replacing the old one. This approach ensures consistency, reduces configuration drift, and simplifies rollback processes.   
Mutable Infrastructure, on the other hand, allows for changes and updates to be made directly to the existing servers or components after they have been deployed. This traditional approach enables quick fixes and updates but can lead to inconsistencies over time as changes accumulate, making it harder to manage and maintain the infrastructure.

# What are the different types of docker network & which is default network?
Docker supports several types of networks to facilitate communication between containers and the outside world. The main types of Docker networks are:
1. Bridge Network: This is the default network type for Docker containers. It creates a private internal network on the host machine, allowing containers to communicate with each other while being isolated from the host's network. Containers on the same bridge network can communicate using IP addresses or container names.
2. Host Network: In this mode, a container shares the host's network stack. This means that the container can use the host's IP address and ports directly, which can improve performance but reduces isolation between the container and the host.
3. Overlay Network: This network type allows containers running on different Docker hosts to communicate securely. It is commonly used in Docker Swarm and Kubernetes environments to enable multi-host networking.
4. Macvlan Network: This network type allows you to assign a MAC address to a container, making it appear as a physical device on the network. This is useful for scenarios where containers need to be directly accessible on the local network.
5. None Network: This mode disables all networking for a container. The container will not have any network interfaces, effectively isolating it from any network communication.
The default network type in Docker is the Bridge Network.


# Akshat Course
You can use Different name as well instead of Dockerfile
To avoid layers of images we use multiple commands in RUN with &&

Alpine - Very Small in size linux based image (yum package)
SLIM   - Larger than alpine, Most Used for Production 
distroless - Very Secure, No shell, No Package Manger, very small attack surface, debug is difficult.

# Optmization of Docker - 
Use Multi stage build
Use alpine or slim for small base image
Combine Multple RUN commands
Use .dockerignore to remove unnecessary files.
RUn as a non-root user
Clean package cache
Remove unnecessary packages
Copy only the required files



ENTRYPOINT ["java", "-jar", "app.jar"] 

## OOM killed
docker ps -a
docker inspect
docker stats         --> Check utilization
Check appln logs
Memory leak for large file loading

cgroup1 v1 details will be in below file once you limit :
cgroup is control group
cat /sys/fs/cgroup/memory/memory.limit_in_bytes

# get the PID of container from below command
docker inspect -f '{{.State.Pid}}' <ContainerName>
ls -l /proc/9837/ns  ---> namespace for PID

> Which namespace does Docker use by default?

<<<<<<< HEAD

# Docker commands - 
docker run -d <imageName>
docker images 
docker ps
docker ps -a 
docker logs <container ID>
docker run  <imageName> whoami ----> To check how docker container is running with which user
=======
# Docker Compose
Docker Compose is a tool for defining and running multi-container Docker applications. 
Instead of running complex, individual docker run commands for your frontend, backend, and database, you describe your entire application stack in a single configuration file.
With a single command, Docker Compose automatically creates, links, and starts all the services, networks, and storage volumes your application needs.

#  How it Works
1. Define the environment: Create a Dockerfile for each separate service so it can be reproduced anywhere.
2. Define the application stack: Outline your services, ports, environment variables, and volumes inside a single configuration file named compose.yaml (or docker-compose.yml).
3. Run the application: Execute docker compose up in your terminal to build and start your entire app seamlessly.


🌟 Key BenefitsSingle-command control: 
Start (docker compose up) and stop (docker compose down) your entire environment instantly.
Isolated networks: Compose automatically creates a single, default network for your services so they can talk to each other safely using just their container names.
Environment consistency: Ensures that every developer on a team runs the exact same setup on their local machine, eliminating the "works on my machine" problem.
Data persistence: Automatically preserves volume data so your database contents aren't lost when containers are stopped.

📄 Example of a Compose FileBelow is a simple example of a compose.yaml file that links a web application to a PostgreSQL database:
services:
  web:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: secret_password


🚀 Core CLI Commands
docker compose up - Starts all containers in the foreground. Use -d to run them in the background.
docker compose down - Stops all running containers and removes networks.
docker compose ps - Lists the current status of the containers in your stack.
docker compose logs - Streams the log outputs from all running services simultaneously.


# Clear and Rerun
docker-compose down -v
docker-compose down -v
docker system prune -f
docker-compose up --build -d
docker compose logs mysql

docker logs dream-vacation-mysql

To restart only frontend - docker compose up -d --no-deps --build frontend
cd /home/azureuser/Dream-Vacation-App/
docker compose up -d --no-deps --build backend


# To check logs
docker compose logs -f backend
docker compose exec -it mysql mysql -u root -pyourdbpassword
docker compose logs -f frontend

Error1:
Creating dream-vacation-mysql ... done
Creating dream-vacation-backend ...
Creating dream-vacation-backend ... error

ERROR: for dream-vacation-backend  Cannot start service backend: failed to set up container networking: driver failed programming external connectivity on endpoint dream-vacation-backend (ae67c55a31c8697ddff734810272e78c29bdb236389645e5f4f7646894360192): failed to bind host port 0.0.0.0:3001/tcp: address already in use
Check - sudo lsof -i :3001

1. Time-out error during frontend npm install process
npm ERR! code ERR_SOCKET_TIMEOUT
npm ERR! network Socket timeout
Even though your internet is working fine, this happens surprisingly often on frontend apps due to the larger number of nested dependencies, especially with React (react-scriptsetc.).

Run - npm install --fetch-retries=5 --fetch-retry-mintimeout=20000 --fetch-retry-maxtimeout=60000

2. Access denied for user ‘root’@’localhost’
Error ensuring table "destinations": Access denied for user 'root'@'localhost'
Ensure your DB-USER is set to the right username if it isn’t “root” or grant the root user the right privileges to read/write to your database.

Login to sudo mysql -u root -p
CHeck for privilegese- SHOW GRANTS FOR 'root'@'localhost';

If root doesn't have access to the DB you're trying to use, grant access like this (from within MySQL):

GRANT ALL PRIVILEGES ON your_database.* TO 'root'@'localhost';
FLUSH PRIVILEGES;


3. Permission denied for Docker daemon
Error2:permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock

Your current user doesn’t have permission to access Docker without sudo.

Solution - Run this command
sudo usermod -aG docker $USER
Check with - docker ps
>>>>>>> cc1272c8dd488efc64be093624df66b477679c49
