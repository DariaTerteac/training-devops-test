## Task 
Create a simple Python program that prints "Hello" and takes a "name" parameter during `run` stage. Create a Dockerfile, build an image and run the container.

## Prerequisites
- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Navigate to the necessary folder
```
cd $HOME/training-devops-solutions/docker/03-hello-world-python-in-docker/src
```

## How to run
[Build](https://docs.docker.com/engine/reference/commandline/build/) the image
```
docker build -t 03-hello-world-python-in-docker:0.1 -f Dockerfile .
``` 

Get [list](https://docs.docker.com/engine/reference/commandline/images/) of all images to check if `03-hello-world-python-in-docker` was created
```
docker images
```
[Run](https://docs.docker.com/engine/reference/commandline/run/) the container with desired name parameter
```
docker run --name 03-task 03-hello-world-python-in-docker:0.1 --name world
```

Please execute the clean up
```
docker rmi -f 03-hello-world-python-in-docker:0.1
docker rm 03-task
```
## References

 1. [Get Started with Docker](https://www.docker.com/get-started/)
 2. [Dockerfile](https://docs.docker.com/engine/reference/builder/#:~:text=A%20Dockerfile%20is%20a%20text,can%20use%20in%20a%20Dockerfile%20.)
 3. [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
 4. [Docker Hub Container Image Library](https://hub.docker.com/)
 5. [docker images](https://docs.docker.com/engine/reference/commandline/images/)
 6. [docker build](https://docs.docker.com/engine/reference/commandline/build/)
 7. [docker run](https://docs.docker.com/engine/reference/commandline/run/)
