
 $ docker image ls ---> To check the images available in your local system.
 $ docker ps ---> To check the running containers in your local system.
 $ docker container ls ---> To check the running containers in your local system.

 Copy the index.html to the running Nginx container:
 $ docker cp index.html nginx-container:/usr/share/nginx/html/index.html

 $ docker exec -it nginx-container /bin/bash  
 -it Combines -i (interactive) and -t (pseudo-TTY), 



 $ docker build -t nginx_app .
    -t flag is used to tag the image with a name (nginx_app in this case).
 $ docker images
 $ docker tag nginx_app nginx_app:ver1

 Run the Container Using the Built NGINX Docker Image:
 $ docker run -d --name nginx_container_1 -p 8080:80 nginx_app:ver1
 -p 8080:80 Maps port 8080 on the host to port 80 on the container, allowing access to the container's web server via localhost:8080.

 $ docker ps
 $ curl http://localhost:8080


 $ docker run -it -d -p 8080:80  --name my-ubuntu-container ubuntu:latest
 $ docker images
 $ docker ps
 $ docker container stop my-ubuntu-container
 $ docker ps -a
 $ docker container start my-ubuntu-container
 $ docker container ls
 $ docker container stop my-ubuntu-container
 $ docker container rm my-ubuntu-container

 $ docker rmi -f <image_id>
 Remove ALL unused images:Deletes any image that is not currently actively running in a container. $ docker image prune -a


 $ docker run -d --name nginx_container -p 8080:80 nginx_app:latest

$ docker ps
$ docker inspect nginx_container       ---> To get detailed information about the container, including its configuration, network settings, and more.

To view specific details like the container's status, use the --format option:

 $ docker inspect --format='{{.State.Status}}' nginx_container

 --format='{{.State.Status}}' This option filters the output to only show the status of the container (such as running and exited).

 To check the logs for troubleshooting or identifying issues, use the following command:
 $ docker logs nginx_container

To monitor real-time resource usage of all running containers, including CPU, memory, network I/O, and disk I/O, use the following command:

 $ docker stats

 Now monitor the resources of a specific container, such as nginx_container, use the following command:

 $ docker stats nginx_container 