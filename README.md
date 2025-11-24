# Docker



A one physical machine are capital intensive 

Hypervisor is a software of virtualization.
ESXI is a software from VMware for virtualization.
Hyper-v is a software from Microsoftfor virtualization.


# Container -
A container means a package that needed things in place & shipping it.
1. Container is a package with just application, application libraries & the OS Modules.
2. If a container is working on my machines, ut should also work on the other machines.
3. With containers you dont spend too much on the hardware.
4. With containers, you can start the container in leass than 1 sec & can stop the conatiner in sec

Serverless means your not responsible for managing the servers.Cloud provider is the responsible for managing the server.

## How to run the conatiners?
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
3.Supports a very HIGH level features.

Docker-CE (Actual docker, use by companies)
Docker (it has podman which is offered by Redhat)

############################################################################################


## Containerization - 
1. Build own containers using the base images from docker hub.
2. lets run them on the top of our servers.
 
A process to make your own images by taking the references as base images is called as containerization.

# To make our images, we need to understand how it works?

> Dockerfile (This is the file where we are going to place all the instructions & this works in a Instruction Argument approach)
> Dockerfile is the only file name for Docker containers, there is no extension for the file.

> dockerfile commands -  https://docs.docker.com/reference/dockerfile/



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
You cannot make chnages on a container, even if you make you would lose them & you have to make the changes on the image.

Immutable which cannot be changed
Mutable which can be changed.

Containers are not like OS, they are meant to run a single process at a time.

Stopping the  container means killing the container
When you START the container means it will create a brand new Container

> Our underline server will run on different network
> Also Containers run on different network

# Overlay Network enables the communication between server network & container network.


# Docker images directory - /var/lib/docker
# Podman images directory - /var/lib/containers/storage

# you can see containers on Standard USer -  $HOME/.local/share/containers/storage/overlay



############################################################################################

##what is the command to install docker edition on linux server rhel 9

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

## DOcker Port

sudo docker run -d -P nginx:latest
-P means Randomly allocates the port


sudo docker run -d -P 80:80 nginx:latest
--docker process runs on 80 redirect to port 80 for internet

Command to enter into conatiner
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

CMD & ENTRYPOINT are a kind of startup for container

Values in ENTRYPOINT cannot be overriden
Values in CMD can be Overriden

1. you can have n number of CMD, but only the latest will be considered in the Dockerfile
2. CMD is typically used to pass the arguments (that means values mentioned in CMD can be overriden)
3. Values in ENTRYPOINT cannot be overriden

Ex - 
$ ls -ltr
ls means ENTRYPOINT, -ltr means CMD

In docker, if you mention any process by in a JSON format, it will be process of its own where the parent process id directly 1 which is the system process.


## Docker containers stores at - 
/var/lib/containers/storage/overlay-containers/

## How to publish the Docker images to docker hub?

$ docker login docker.io
--> enter your username & pwd

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

sudo lvmextend -l +50%FREE /dev/mapper/RootVG-homeVol
sudo lvmextend -l +100%FREE /dev/mapper/RootVG-homeVol
sudo lvextend -r -L +6G  /dev/mapper/RootVG-homeVol

4. Expand the FileSystem
sudo xfs_growfs /var
sudo xfs_growfs /home

5. Check with
df -Tkh


######## Docker Volumes Mapping ####
1. Containers are ephermal & hence we lose the data if the container is deleted.
2. In cases, we need to have the data persistent even if we lost or remove the container.
3. A volume of host machine can be mapped to a container using -v option.

-v <hostPath>:<containerPath>


##### StateFul Vs Stateless #########

Apps are of 2 types - Stateful & Stateless 

Apps which are dependent on storage or data are called as stateful appln
Apps which are NOT dependent on storage or data are called as stateless appln


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


> Dockerfile ---Build--->>  Docker Image ----Run--->> Docker Container


# 
1. Through Terraform Create Instance
2. Through Ansible Deploy the docker


> No one runs the container on EC2 instance, instead they uses Container Orchestration tools.


## Challenges with Running Containers on VM's?
 1. We are responsible for managing & manitaining the underlying infrastructure.
 2. We are responsible for manitaining the container runtime.
 3. NETWORK is not tightly coupled, what if you have more than 3 containers of frontend, 3 containers of backend, how can you communicate? 
 4. If containers are running on independent machines, how to achieve the shared STORAGE concept?

Main problems are Common Network & SHared Storage

 > TO overcome above challenges, we can use K8s Container Orchestration Tool.



 > Amazon Prime moved from microservices to monolith - its a prie example of moving containers to VM's
It depends on what your application required.
