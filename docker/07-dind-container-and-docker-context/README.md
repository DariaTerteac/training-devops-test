## Task 
Run Docker-in-Docker container and find out how to switch between contexts.

## Prerequisites

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

## How to run

- Firstly, run `ubuntu` based container, we will need it for check

```
docker run ubuntu
```

- Run a dind container

```
docker run --privileged -d -p 1234:2375 --name 07-docker-in-docker -e DOCKER_TLS_CERTDIR="" docker:23.0.1-dind
```

- Create new docker context
```
docker context create dind-context --docker "host=http://localhost:1234"
```

- Get the list of contexts
```
docker context ls
```

- Switch to the newly created context
```
docker context use dind-context
```

- Validate that there are no images in the newly created context `dind-context`

```
docker images
```

- Switch to the default context
```
docker context use default
```

- Validate that there are at least two images in the `default` context:
    - `ubuntu:latest`, after running a container in the very beginning
    - `docker:23.0.1-dind`, after running dind container

```
docker images
```

Please execute the cleanup

```
docker rm -f $(docker ps -aq) 
docker rmi -f $(docker images -q)    
docker context rm dind-context
docker system prune
```

## References
 1. [Get Started with Docker](https://www.docker.com/get-started/)
 2. [docker images](https://docs.docker.com/engine/reference/commandline/images/)
 3. [docker build](https://docs.docker.com/engine/reference/commandline/build/)
 4. [docker run](https://docs.docker.com/engine/reference/commandline/run/)
 5. [Docker in Docker](https://shisho.dev/blog/posts/docker-in-docker/)
 6. [Docker context](https://docs.docker.com/engine/context/working-with-contexts/)