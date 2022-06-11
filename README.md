# Docker and Kubernetes
During my learning I kept track of all the important commands for Docker and Kubernetes.

**Feel free to add more commands to this File**

## Docker Cheatbook
### Docker CLI
1. docker run image_name : to run specified docker image.
2. docker run image_name command : extension to above with command parameter override the default
3. docker ps : list all running containers
4. docker ps --all : list all containers
5. docker create image_name : create a container
6. docker start container_id : start/restart the container, container_id can be found with command 3,4 and 5
7. docker start -a container_id : prints the output from the container
8. docker system prune : remove stopped containers
9. docker logs container_id : log outputs
10. docker stop container_id : stops the container
11. docker kill container_id : kill the container
12. docker exec -it container_id command : allows input to the existing container id directly
13. docker exec -it container_id sh : get a command prompt
14. CTRL + C or CTRL + D or exit : To exit current window
15. docker build . : build the image from a docker file
16. docker build -t testbuild:latest . : build the image with a tag specified after tag file
17. docker commit -c start_command container_id : creates a new image from the current container with specified start command
18. docker run -p source_port : container_port image_id : points the local network port to container port

### Docker Compose - Fast and Multicommands alternative to Docker CLI
1. docker-compose : avoid repetitive tasks with fast and multicommands
2. docker-compose up : equivalent to docker run image_id
3. docker-compose up --build : equivalent to docker build and run together
4. docker-compose up -d : containers running in the background
5. docker-compose down : to stop all the containers
6. restart policies : "no", always, on-failure, unless-stopped
7. docker-compose ps : to find all the running containers

### Docker File Keywords
1. FROM
2. RUN
3. CMD
4. COPY
5. WORKDIR
