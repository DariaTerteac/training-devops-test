## Task 
Run [testcontainers-java](https://github.com/testcontainers/testcontainers-java) (redis-backed-cache-testng example).
Find out how to control the sequence of running containers with sleep & dockerize.

## Prerequisites
- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Copy necessary submodule to this task

```
cp -r $HOME/training-devops-solutions/submodules/testcontainers-java $HOME/training-devops-solutions/docker/09-control-sequence-of-running-containers/src
```

- Navigate to the necessary folder
```
cd $HOME/training-devops-solutions//docker/09-control-sequence-of-running-containers/src
```

## How to run

```
docker network create demo-network
```

```
docker run --privileged -dti --name dind-container -e DOCKER_TLS_CERTDIR="" --network demo-network docker:23.0.1-dind
```

The first example, simply run testcontainers-java:

```
docker build -t 09-testcontainers-java:0.1 -f Dockerfile .
docker run --name 09-task --network demo-network 09-testcontainers-java:0.1
```

The second example, run testcontainers-java with sleep:

```
docker build -t 09-testcontainers-java-sleep:0.1 -f Dockerfile-sleep .
docker run --name 09-task-sleep --network demo-network 09-testcontainers-java-sleep:0.1
```

The third example, run testcontainers-java with dockerize:
```
docker build -t 09-testcontainers-java-dockerize:0.1 -f Dockerfile-dockerize .
docker run --name 09-task-dockerize --network demo-network 09-testcontainers-java-dockerize:0.1
```

Please execute the clean up
```
docker rmi -f 09-testcontainers-java:0.1
docker rmi -f 09-testcontainers-java-sleep:0.1
docker rmi -f 09-testcontainers-java-dockerize:0.1
docker rm 09-task
docker rm 09-task-sleep
docker rm 09-task-dockerize
docker rm -f dind-container
docker network prune -f
```

## References

 1. [Get Started with Docker](https://www.docker.com/get-started/)
 2. [Dockerfile](https://docs.docker.com/engine/reference/builder/#:~:text=A%20Dockerfile%20is%20a%20text,can%20use%20in%20a%20Dockerfile%20.)
 3. [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
 4. [docker images](https://docs.docker.com/engine/reference/commandline/images/)
 5. [docker build](https://docs.docker.com/engine/reference/commandline/build/)
 6. [docker run](https://docs.docker.com/engine/reference/commandline/run/)
