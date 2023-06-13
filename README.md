# Deployed-Containers
This repo is used to develop templates to containerize applications using Docker and the respective deployment process using Kubernetes
<br /><br />
# Docker
Download Docker App (w/ UI or through terminal) from here: https://docs.docker.com/desktop/install/mac-install/
Check to see if you have docker installed: 
> docker version
Have Docker App open and running
## Containerized NodeJS App
This folder contains a simple NodeJS app and a Dockerfile that will execute the app in a container which would then be accessible local browsers.
- Command structure: docker build -t [myImageName]:[tag] [locationOfDockerfile]
```md 
cd ContianerizedNodeJSApp
docker build -t my-nodeapp:1.0 . 
```
- Show the image that we just built
```md 
docker images 
```
- Run our image (our node is listening on port 3000, we'll bind it to port 4000)
```md 
docker run -d -p 4000:3000 my-nodeapp:1.0
```
- Show running containers
```md 
docker ps
```
- See our containerized running app on http://localhost:4000/
- Show terminal output
```md 
docker log [containerID]
```
> app listening on port 3000

## Containerized Java App
This folder contains a simple Java app and a Dockerfile that will execute the Java code in a container
```md
cd ContainerizedJavaApp
```
```md
docker build -t java-app .
```
```md
docker run java-app
```
# Kubernetes
This folder contains yaml files to deploy a web-app pod which will connect to the mongoDB pod through internal service using (external) configuration data from config map and secret (to securely send DB url and credentials). It will also make the web-app accessible externally from the browser using external service. 
Docker App must be open and running.
- Install Minikube
```md
brew install minikube 
```
```md
minikube start --driver=docker
```
```md
minikube status
```
- ConfigMap and secret must exist before Deployments
> kubectl apply -f <filename.yaml>
- apply manages applications through files defining K8s resources
```md
kubectl apply -f mongo-config.yaml
```
```md
kubectl apply -f mongo-secret.yaml
```
```md
kubectl apply -f mongo.yaml
```
```md
kubectl apply -f webapp.yaml
```
```md
kubectl get all
```
Output = [list of pods], [list of services], [list of deployments]
```md
kubectl get configmap
```
```md
kubectl get secret
```
```md
kubectl get pod
```
```md
kubectl describe pod <name-of-pod-from-above-cmd>
```
```md
kubectl describe service webapp-service
```
- View logs of container:
```md
kubectl logs <podName>
```
- View web app in browser
```md
kubectl get svc
```
- NodePort Service is accessible on each worker node's IP address. you can get internalIP from: 
```md
kubectl get node -o wide
```
- Or get Minikube's IP addr
```md
minikube ip
```
Now go to browser and type: [ip_addr_found_above]:30100
#### !Note: Database is also connected and works
