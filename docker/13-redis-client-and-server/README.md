## Task
Run redis server & client in docker. Make accessible the database to the host user after the container stops. Check if data persists in another redis container.

## Prerequisites

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

## How to run

- Create network

```
docker network create redis-network
```

- Create `data` folder in `$HOME/training-devops-solutions/docker/13-redis-client-and-server/`

```
mkdir -p $HOME/training-devops-solutions/docker/13-redis-client-and-server/data
```

- Navigate to the necessary folder

```
cd $HOME/training-devops-solutions/docker/13-redis-client-and-server/src
```

- Build the image

```
docker build -t 13-redis:0.1 --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) -f Dockerfile .
```

- Run server container in detached mode

```
docker run -d --network redis-network --name redis-server --mount type=bind,source=$HOME/training-devops-solutions/docker/13-redis-client-and-server/data,target=/data 13-redis:0.1
```

- Run client container

```
docker run -it --name redis-client --network redis-network redis redis-cli -h redis-server
```

- Run the following commands in the `Redis Client` container:

```
SET key "Hello Redis"
```
```
GET key
```
```
save
```

```
exit
```

- Navigate to the volume folder

```
cd $HOME/training-devops-solutions/docker/13-redis-client-and-server/data
```

- Check if `dump.rdb` file is present

```
ls -lha
```

- Check if the content of the file is available for another container

- Run a new container called `redis-server-test`
```
docker run -d --network redis-network --name redis-server-test 13-redis:0.1
```
- Copy received file in container
```
docker cp $HOME/training-devops-solutions/docker/13-redis-client-and-server/data/dump.rdb redis-server-test:/data
```
- Enter the container's shell and make sure the file is present
```
docker exec -it redis-server-test sh
```
```
ls
```
```
exit
```
- Stop and start the container
```
docker stop redis-server-test
```
```
docker start redis-server-test
```
- Enter the shell again and try to reach the necessary information
```
docker exec -it redis-server-test sh
```
```
ls
```
```
redis-cli
```
```
get key
```
```
exit
```
```
exit
```

- Please execute the cleanup

```
docker network prune -f
```
```
docker rm -f $(docker ps -aq)
```
```
docker rmi -f $(docker images -q)
```
```
docker system prune
```

## References

 1. [Get Started with Docker](https://www.docker.com/get-started/)
 2. [Dockerfile](https://docs.docker.com/engine/reference/builder/#:~:text=A%20Dockerfile%20is%20a%20text,can%20use%20in%20a%20Dockerfile%20.)
 3. [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
 4. [docker images](https://docs.docker.com/engine/reference/commandline/images/)
 5. [docker build](https://docs.docker.com/engine/reference/commandline/build/)
 6. [docker run](https://docs.docker.com/engine/reference/commandline/run/)
 7. [How to Use the Redis Docker Official Image](https://www.docker.com/blog/how-to-use-the-redis-docker-official-image/)
 8. [Bind mounts](https://docs.docker.com/storage/bind-mounts/)
