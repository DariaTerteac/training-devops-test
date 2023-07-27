## Task 
Run two containers from ubuntu image. The first container writes a string "content of test file" in test1.txt file located on the volume. The second container reads this file and prints file content in the console

## Prerequisites

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

## How to run

- Navigate to the necessary folder

```
cd $HOME/training-devops-solutions/docker/06-interconnection-via-volume/src
```

- Build images

```
docker build -t 06-writer-container:0.1 -f Dockerfile.writer .
docker build -t 06-reader-container:0.1 -f Dockerfile.reader .  
```

- Create a volume

```
docker volume create shared-volume  
```

- Run the two containers and connect them through a shared volume.
```
docker run -v shared-volume:/data --name 06-writer 06-writer-container:0.1
docker run -v shared-volume:/data --name 06-reader 06-reader-container:0.1
```
- The output should be `content of test file`

Please execute the clean up

```
docker rmi -f 06-writer-container:0.1
docker rmi -f 06-reader-container:0.1
docker rm -f 06-writer
docker rm -f 06-reader
docker system prune
```
## References
1. [Get Started with Docker](https://www.docker.com/get-started/)
2. [Dockerfile](https://docs.docker.com/engine/reference/builder/#:~:text=A%20Dockerfile%20is%20a%20text,can%20use%20in%20a%20Dockerfile%20.)
3. [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
4. [docker build](https://docs.docker.com/engine/reference/commandline/build/)
5. [docker run](https://docs.docker.com/engine/reference/commandline/run/) 
6. [docker volume](https://docs.docker.com/storage/volumes/)
7. [Mounting a Data Volume](https://phoenixnap.com/kb/docker-volumes)