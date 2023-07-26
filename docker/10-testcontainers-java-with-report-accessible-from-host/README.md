## Task

Run [testcontainers-java](https://github.com/testcontainers/testcontainers-java) (redis-backed-cache-testng example) in Docker using [dockerize](https://github.com/powerman/dockerize) and make reports after running the tests accessible to the host machine user after the container is stopped.

## Prerequisites

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

- Copy necessary submodule inside `src` folder

```
cp -r $HOME/training-devops-solutions/submodules/testcontainers-java $HOME/training-devops-solutions/docker/10-testcontainers-java-with-report-accessible-from-host/src
```

- Navigate to the necessary folder

```
$HOME/training-devops-solutions/docker/10-testcontainers-java-with-report-accessible-from-host/src
```

## How to run

- Create a new docker network:

```
docker network create demo-network
```

- Run a dind container

```
docker run --privileged -dti --name dind-container -e DOCKER_TLS_CERTDIR="" --network demo-network docker:23.0.1-dind
```

- Build the image

```
docker build -t 10-testcontainers-java-with-report-accessible-from-host:0.1 \
--build-arg USER_ID=$(id -u) \
--build-arg GROUP_ID=$(id -g) \
-f Dockerfile .
```

- Create new folder called `report` in `$HOME/training-devops-solutions/docker/10-testcontainers-java-with-report-accessible-from-host/`

```
mkdir $HOME/training-devops-solutions/docker/10-testcontainers-java-with-report-accessible-from-host/report
```

- Run `10-task` container with testcontainers-java

```
docker run -it --name 10-task \
--mount type=bind,source=$HOME/training-devops-solutions/docker/10-testcontainers-java-with-report-accessible-from-host/report,destination=/app/examples/redis-backed-cache-testng/build \
--network demo-network \
10-testcontainers-java-with-report-accessible-from-host:0.1
```

Change the directory to `$HOME/training-devops-solutions/docker/10-testcontainers-java-with-report-accessible-from-host/report/reports/tests/test/` folder on the host to check the report.

```
$HOME/training-devops-solutions/docker/10-testcontainers-java-with-report-accessible-from-host/report/reports/tests/test
```

Please execute the cleanup:

```
docker rmi -f 10-testcontainers-java-with-report-accessible-from-host:0.1
```

```
docker rm 10-task
```

```
docker rm -f dind-container
```

```
docker network rm demo-network
```

## References

1.  [Get Started with Docker](https://www.docker.com/get-started/)
2.  [Dockerfile](https://docs.docker.com/engine/reference/builder/#:~:text=A%20Dockerfile%20is%20a%20text,can%20use%20in%20a%20Dockerfile%20.)
3.  [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
4.  [docker images](https://docs.docker.com/engine/reference/commandline/images/)
5.  [docker build](https://docs.docker.com/engine/reference/commandline/build/)
6.  [docker run](https://docs.docker.com/engine/reference/commandline/run/)
