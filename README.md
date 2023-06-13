# Learning-Ansible
This folder is used for learning about Ansible and how to design Ansible Playbooks

# GitHub-Actions
This folder is used to learn and design various GitHub Action workflows using a sample Gradle-Java app. Go to .github/workflows to view all the workflows.

##### build the project

    ./gradlew build

##### build Docker image called java-app. Execute from root

    docker build -t java-app .
    
##### push image to repo 

    docker tag java-app demo-app:java-1.0

# Learning-Jenkins
This folder is used for learning about how to prepare Jenkinsfile and how to work with Jenkins
## Build the Jenkins BlueOcean Docker Image
```
docker build -t myjenkins-blueocean:2.332.3-1 .
```

IF you are having problems building the image yourself, you can pull from my registry and rename it to `myjenkins-blueocean:2.332.3-1`
```
docker pull devopsjourney1/jenkins-blueocean:2.332.3-1 && docker tag devopsjourney1/jenkins-blueocean:2.332.3-1 myjenkins-blueocean:2.332.3-1
```

## Create the network 'jenkins'
```
docker network create jenkins
```

## Run the Container
### MacOS / Linux
```
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.332.3-1
```

### Windows
```
docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.332.3-1
```


## Get the Password
```
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

## Connect to the Jenkins
```
https://localhost:8080/
```

## Installation Reference:
https://www.jenkins.io/doc/book/installing/docker/


## alpine/socat container to forward traffic from Jenkins to Docker Desktop on Host Machine

https://stackoverflow.com/questions/47709208/how-to-find-docker-host-uri-to-be-used-in-jenkins-docker-plugin
```
docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
```
From the output of above command, execute below command to get the ip address
```
docker inspect <container_name>
```

## Use Jenkins Python Agent
```
docker pull devopsjourney1/myjenkinsagents:python
```


# Container-Deployments
This repo is used to develop templates to containerize applications using Docker and the respective deployment process using Kubernetes
<br /><br />
## Docker
Download Docker App (w/ UI or through terminal) from here: https://docs.docker.com/desktop/install/mac-install/
Check to see if you have docker installed: 
> docker version
Have Docker App open and running
### Containerized NodeJS App
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

### Containerized Java App
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
## Kubernetes
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

