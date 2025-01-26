# Docker and Kubernetes

During my Udemy course, I kept track here of all the important commands for Docker and Kubernetes.

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
18. docker run -p source_port:container_port <image:id> : container_port image_id : points the local network port to container port
19. docker build -f Dockerfile.dev . : Specify specific docker file to run from.
20. docker run reference1 reference2 image_id :

### Docker Compose - Fast and Multi-commands alternative to Docker CLI

1. docker-compose : avoid repetitive tasks with fast and multi-commands
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

### Creating a Production-Grade Workflow

See frontend_example. Things we did -

1. Divided docker file for dev and production version separately.
2. To reflect the development changes immediately. We can use docker volumes.

```bash
docker run -it -p 3000:3000 -v /app/node_modules -v ${PWD}:/app frontend
```

## Kubernetes

### Commands

- Kubectl apply -f file_name : apply the configuration file
- Kubectl get pods : list all the pods
- Kubectl get services : list all the services
- kubectl describe object_type object_name : describe the object
- kubectl delete -f file_name : delete the object from the configuration file
- kubectl apply -f deployment_file_name : apply the deployment configuration file
- kubectl get deployments : list all the deployments
- kubectl get pods -o wide : list all the pods with more details
- kubectl set image object_type / object_name container_name=image_name : update the image of the deployment
- kubectl apply -f folder_name : apply all the configuration files in the folder
- kubectl logs pod_name : get the logs of the pod
- kubectl get storageclass : list all the storage classes
- kubectl get pv : list all the persistent volumes
- kubectl get pvc : list all the persistent volume claims
- kubectl create secret generic secret_name --from-literal key=value : create a secret
- kubectl get secrets : list all the secrets
  
### Imperative vs Declarative Deployment

- Imperative : Do exactly these steps to arrive at this container setup
- Declarative : Our container setup should look like this, make it happen. Only 4 properties can be changed in pods. 

### Deployment

Manages a set of identical pods, ensuring that they have the correct config and that the right number exists. You can merge the deployment and service file into one file. Check the example in the k8s folder, server-config.yaml.

### Volumes

An object that allows a container to store data at the pod level. So, if the pod crashes the data is lost.
PVC (Persistent Volume Claim) is a request for storage by a pod. PV (Persistent Volume) is a piece of storage in the cluster, not affected by failure of pod. PVC can request a specific size and access mode.

### Services

#### ClusterIP Service

Exposes a set of pods to other objects in the cluster. Not exposed to the outside world.

#### NodePort Service

Exposes a set of pods to the outside world. A node port is opened on each node and is forwarded to the clusterIP.

#### LoadBalancer Service

Legacy way of getting network traffic into a cluster. Only give access to one service - ClusterIP. Exposes a set of pods to the outside world. Only available when you're using a cloud provider.

#### Ingress Service

New way of doing things. Can do multiple services at same time. Exposes a set of services to the outside world. Only available when you're using a cloud provider.
