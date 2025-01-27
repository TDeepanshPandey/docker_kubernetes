# Docker and Kubernetes

During my Udemy course, I kept track here of all the important commands for Docker and Kubernetes.

**Feel free to add more commands to this File**

## Docker Cheatbook

### Docker CLI

1. *docker run image_name* : to run specified docker image.
2. *docker run image_name command* : extension to above with command parameter override the default
3. *docker ps* : list all running containers
4. *docker ps --all* : list all containers
5. *docker create image_name* : create a container
6. *docker start container_id* : start/restart the container, container_id can be found with command 3,4 and 5
7. *docker start -a container_id* : prints the output from the container
8. *docker system prune* : remove stopped containers
9. *docker logs container_id* : log outputs
10. *docker stop container_id* : stops the container
11. *docker kill container_id* : kill the container
12. *docker exec -it container_id command* : allows input to the existing container id directly
13. *docker exec -it container_id sh* : get a command prompt
14. *CTRL + C or CTRL + D or exit* : To exit current window
15. *docker build .* : build the image from a docker file
16. *docker build -t testbuild:latest .* : build the image with a tag specified after tag file
17. *docker commit -c start_command container_id* : creates a new image from the current container with specified start command
18. *docker run -p source_port:container_port <image:id>* : container_port image_id : points the local network port to container port
19. *docker build -f Dockerfile.dev .* : Specify specific docker file to run from.
20. *docker run reference1 reference2 image_id* : to run the image with multiple commands

### Docker Compose - Fast and Multi-commands alternative to Docker CLI

1. *docker-compose* : avoid repetitive tasks with fast and multi-commands
2. *docker-compose up* : equivalent to docker run image_id
3. *docker-compose up --build* : equivalent to docker build and run together
4. *docker-compose up -d* : containers running in the background
5. *docker-compose down* : to stop all the containers
6. *restart policies* : "no", always, on-failure, unless-stopped
7. *docker-compose ps* : to find all the running containers

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

- *Kubectl apply -f file_name* : apply the configuration file
- *Kubectl get pods* : list all the pods
- *Kubectl get services* : list all the services
- *kubectl describe object_type object_name* : describe the object
- *kubectl delete -f file_name* : delete the object from the configuration file
- *kubectl apply -f deployment_file_name* : apply the deployment configuration file
- *kubectl get deployments* : list all the deployments
- *kubectl get pods -o wide* : list all the pods with more details
- *kubectl set image object_type / object_name container_name=image_name* : update the image of the deployment
- *kubectl apply -f folder_name* : apply all the configuration files in the folder
- *kubectl logs pod_name* : get the logs of the pod
- *kubectl get storageclass* : list all the storage classes
- *kubectl get pv* : list all the persistent volumes
- *kubectl get pvc* : list all the persistent volume claims
- *kubectl create secret generic secret_name --from-literal key=value* : create a secret
- *kubectl get secrets* : list all the secrets
  
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

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.0/deploy/static/provider/cloud/deploy.yaml

kubectl -n ingress-nginx get pod -o yaml

kubectl delete -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.0/deploy/static/provider/cloud/deploy.yaml
```

## Docker Desktop's Kubernetes Dashboard

If you are using Docker Desktop's built-in Kubernetes, setting up the admin dashboard is going to take a little more work.

1. Install Helm

Following the instructions provided here, run the three following commands in your terminal:
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3

chmod 700 get_helm.sh

./get_helm.sh
```

2. Deploy Kubernetes Dashboard

Run the following two commands in your terminal:

```bash
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/

helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```

3. Create a dash-admin-user.yaml file and paste the following:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
```

4. Apply the dash-admin-user configuration:
   
```bash
kubectl apply -f dash-admin-user.yaml
```

5. Create dash-clusterrole-yaml file and paste the following:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: admin-user
    namespace: kubernetes-dashboard
```

6. Apply the ClusterRole configuration:

```bash
kubectl apply -f dash-clusterrole.yaml
```

7. In the terminal, run:

```bash
kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
```

**Important** - You must keep this terminal window open and the proxy running!

8. Visit the following URL in your browser to access your Dashboard:
https://127.0.0.1:8443/#/login

**Important** - visiting localhost:8443 instead of 127.0.0.1:8443 will result in authentication failure!

9. Obtain the token

In your terminal, run the following command:

```bash
kubectl -n kubernetes-dashboard create token admin-user
```

10. Copy the token from the above output
Copy and paste the token into the login form of the dashboard. Be careful not to copy any extra spaces or output such as the trailing % you may see in your terminal.

11. After a successful login, you should now be redirected to the Kubernetes Dashboard.

The above steps can be found in the official documentation:

[URL](https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md)

## Scaffold

Scaffold is a tool that watches your files for changes and automatically rebuilds your Docker image and restarts your Kubernetes pods. It's a great tool for speeding up your development workflow.

1. Install Scaffold

```bash
choco install scaffold
```

Create a scaffold.yaml file in the root of your project.
