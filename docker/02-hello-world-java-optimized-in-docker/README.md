## Task
Optimize this [Dockerfile](src/Dockerfile) to reduce building time and image size.

## Prerequisites
- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Please get familiar with [Example 01](../01-hello-world-java-in-docker/README.md) where a simple [Dockerfile](src/Dockerfile) is applied

- Navigate to the necessary folder
```
cd $HOME/training-devops-solutions/docker/02-hello-world-java-optimized-in-docker/src
```

## Optimization
The first image will work correctly and execute the necessary task, but let's have a closer look: 

* Image building took ~ 400 seconds.
* Image size is `684 MB`

These values are unacceptable for such a simple application, therefore Dockerfile needs to be improved.

The heaviest part of the Dockerfile was its base image and JDK, but we don't need all Ubuntu properties to print "Hello world!". That's why it is better to choose multistage build that would execute our task.

## How to run
[Build](https://docs.docker.com/engine/reference/commandline/build/) the image
```
docker build -t 02-hello-world-java-in-docker-optimized:0.1 -f ./Dockerfile-optimized .
``` 

Get [list](https://docs.docker.com/engine/reference/commandline/images/) of all images to check the size of the image
```
docker images
```
[Run](https://docs.docker.com/engine/reference/commandline/run/) the container
```
docker run --name 02-task-optimized 02-hello-world-java-in-docker-optimized:0.1
```

As the result: 

* Image building took ~ 5 seconds.
* Image size is `223 MB`

Please execute the clean up
```
docker rmi -f 02-hello-world-java-in-docker-optimized:0.1
docker rm 02-task-optimized
```

## References

 1. [Get Started with Docker](https://www.docker.com/get-started/)
 2. [Dockerfile](https://docs.docker.com/engine/reference/builder/#:~:text=A%20Dockerfile%20is%20a%20text,can%20use%20in%20a%20Dockerfile%20.)
 3. [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
 4. [Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)
 5. [Optimizing Docker Image Sizes](https://taylor.callsen.me/optimizing-docker-image-size/)
 5. [docker images](https://docs.docker.com/engine/reference/commandline/images/)
 6. [docker build](https://docs.docker.com/engine/reference/commandline/build/)
 7. [docker run](https://docs.docker.com/engine/reference/commandline/run/)
