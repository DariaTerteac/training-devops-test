# Training-devops-solutions

## Prerequisites

All commands run on Manjaro operating system.<br>
To configure your workstation click [here](https://github.com/Alliedium/awesome-linux-config/tree/master/manjaro) and follow the instructions.<br>
In the third step please choose [for workstation](https://github.com/Alliedium/awesome-linux-config/tree/master/manjaro#for-workstation) to run this script `./install_all.sh`

Ensure git, Docker and k3d were installed on your machine by running

```
git --version
docker --version
k3d --version
```
### 1. Clone the project:

```
git clone git@github.com:Alliedium/training-devops-dterteac.git $HOME/training-devops-solutions
```

### 2. Navigate to the necessary folder:

```
cd $HOME/training-devops-solutions
```

### 3. Run the command below to connect submodules:

```
git submodule update --init --recursive
```
## Clean up

To clean up the Docker containers, images, and k3d clusters after practicing with the tasks, follow these steps:

- Remove running Docker containers by running the command: 
```
docker rm -f $(docker ps -aq)
```
- Delete Docker images that are no longer needed using the command: 
```
docker rmi $(docker images -q)
```
- Delete the k3d clusters by executing the command (replace <cluster_name> with the actual name of the cluster): 
```
k3d cluster delete <cluster_name> 
```
- Remove any remaining volumes associated with the containers by running: 
```
docker volume prune -f
```
- Optionally, remove any unused networks by executing: 
```
docker network prune -f
```
Please note that these commands will remove all containers, images, clusters, and associated resources, so make sure you have backed up any important data or configurations before proceeding with the clean-up process.


# Docker

## [D-01 Run HelloWorld.java in Docker](docker/01-hello-world-java-in-docker)

- Create a simple Java program that prints "Hello world!". <br>
- Create a Dockerfile, build an image and run the container. <br>

## [D-02 Optimize the Dockerfile from D-01](docker/02-hello-world-java-optimized-in-docker)

- Optimize previously created Dockerfile to make the image faster and lighter. <br>

## [D-03 Run HelloWorld.py in Docker and pass a parameter to it](docker/03-hello-world-python-in-docker)

- Create a simple Python program that prints "Hello" and takes a "name" parameter during run stage.<br>
- Create a Dockerfile, build an image and run the container. <br>

## [D-04 Run HelloWorld.js in Docker](docker/04-hello-world-js-in-docker)

- Create a simple JavaScript program that prints "Hello world!". <br>
- Create a Dockerfile, build an image and run the container. <br>

## [D-05 Interconnect two containers via network](docker/05-interconnection-via-network)
- Interconnect in docker 2 containers: postgres client & postgres server by hostname, not the IP address.

## [D-06 Interconnect two containers via volume](docker/06-interconnection-via-volume)
- Run two containers from ubuntu image.
- The first container writes a string "content of test file" in test1.txt file located on the volume.
- The second container reads this file and prints file content in the console

## [D-07 Run Docker-in-Docker and switch between contexts](docker/07-dind-container-and-docker-context)
- Run Docker-in-Docker container and find out how to switch between contexts.

## [D-08 Run nginxdemos/hello locally and nginxdemos/hello inside of dind & get access to both](docker/08-nginx-locally-and-in-dind)
- Run two containers: nginxdemos/hello inside of dind and nginxdemos/hello in local docker.
- Get access to both of them from the localhost.

## [D-09 Run testcontainers-java in Docker and control the sequence of containers running](docker/09-control-sequence-of-running-containers)

- Run testcontainers-java in Docker
- Control the sequence of containers running using sleep
- Control the sequence of containers running using dockerize

## [D-10 Run testcontainers-java and make test reports available on host](docker/10-testcontainers-java-with-report-accessible-from-host)

- Run testcontainers-java in Docker
- Make report after running tests available on host machine

## [D-11 Run testcontainers-python and make test reports available on host](docker/11-testcontainers-python-with-report-accessible-from-host)

- Run testcontainers-python in Docker
- Make report after running tests available on host machine

## [D-12 Run testcontainers-node and make test reports available on host](docker/12-testcontainers-node-with-report-accessible-from-host)

- Run testcontainers-node in Docker
- Make report after running tests available on host machine

## [D-13 Run redis client & server in docker](docker/13-redis-client-and-server)
- Run redis server & client in docker.
- Make the database accessible to the host user after the container stops.

# Kubernetes

## [K-01 Run optimized HelloWorld.java in k3d](kubernetes/01-hello-world-java-in-k3d)

- Run optimized version of HelloWorld.java in k3d

## [K-02 Run HelloWorld.py in k3d](kubernetes/02-hello-world-python-in-k3d)

- Run simple program HelloWorld.py in k3d

## [K-03 Run HelloWorld.js in k3d](kubernetes/03-hello-world-node-in-k3d)

- Run simple program HelloWorld.js in k3d

## [K-04 Create a deployment with 4 pods from nginxdemos/hello image and provide access to the nginx web page](kubernetes/04-deployment-with-4-pods)

- In k3d cluster using deployment run 4 pods with a container from nginxdemos/hello image.
- Provide access to the nginx web pages from outside of cluster using service.

## [K-05 Create a pod with 2 containers and make them interconnect via volume](kubernetes/05-interconnect-containers-via-volume)

- Run in k3d cluster pod with 2 containers from ubuntu image.
- The first container writes a string "content of test file" in test1.txt file located on the volume.
- The second container reads this file and prints file content in the console.

## [K-06 Create a pod with 2 containers and control the sequence of containers running with 4 different ways](kubernetes/06-control-the-sequence-of-running-containers)

- Control the sequence of containers running [writer-reader example](kubernetes/05-interconnect-containers-via-volume)?

## [K-07 Create a pod with DIND server and DIND client](kubernetes/07-dind-client-and-server)

- Run in k3d cluster pod with 2 containers from images:
  docker:23.0.1-dind (Docker server)
  docker:23.0.1-cli-alpine3.17 (Docker client)
- Go to the container Docker client and connect to the Docker server, run in Docker server nginxdemos/hello image.
- Provide access to nginxdemos/hello web page from outside of the cluster.

## [K-08 Run testcontainers-java in k3d and make test reports available on host after container is stopped](kubernetes/08-testcontainers-java-in-k3d)

- Run testcontainers-java in k3d
- Make report after running tests available on host machine

## [K-09 Run testcontainers-python in k3d and make test reports available on host after container is stopped](kubernetes/09-testcontainers-python-in-k3d)

- Run testcontainers-python in k3d
- Make report after running tests available on host machine

## [K-10 Run testcontainers-node in k3d and make test reports available on host after container is stopped](kubernetes/10-testcontainers-node-in-k3d)

- Run testcontainers-node in k3d
- Make report after running tests available on host machine

## [K-11 Run testcontainers-java in k3d via argo workflows](kubernetes/11-testcontainers-java-argo-workflow)
- Run testcontainers-java in k3d using [Argo Workflows]()

## [K-12 Run a workflow using testcontainers: java, python and node - one test per each and get test reports]()
- Run a workflow with three testcontainers: java, python and node
- Make reports after running tests available on host machine

## [K-13 Run redis server & client in k3d]()
- Run redis server & client in k3d. 

## [K-14 Create 3 deployments with nginxdemos/hello and expose it using 2 services](kubernetes/14-three-deployments-two-services)
- Create 3 deployments with nginxdemos/hello and expose them using 2 services.
- The first  service needs to provide load balancing for the 1st and 2nd deployments
- The second service needs to provide load balancing for the 2nd and 3rd deployments. 

## [K-15 Create a helm chart using springboot application, postgresql and pgadmin]()
- Create a helm chart using springboot application, postgresql and pgadmin.
- Demonstrate that the helm chart deletion and re-installation does not cause data loss.

## [K-16 Parametrize previously created helm chart (with springboot, postgresql and pgadmin)]()
- Parametrize previously created [helm chart](../15-helm-chart) with springboot application, postgresql and pgadmin.
- Demonstrate that the helm chart deletion and re-installation does not cause data loss.
