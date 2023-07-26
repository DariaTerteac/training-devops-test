## Task

Run [testcontainers-python](https://github.com/testcontainers/testcontainers-python) in Docker using [dockerize](https://github.com/powerman/dockerize) and make reports after running the tests accessible to the host machine user after the container is stopped.

## Prerequisites

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

```
cp -r $HOME/training-devops-solutions/submodules/testcontainers-python $HOME/training-devops-solutions/docker/11-testcontainers-python-with-report-accessible-from-host/src
```

- Navigate to the necessary folder

```
$HOME/training-devops-solutions/docker/11-testcontainers-python-with-report-accessible-from-host/src
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
docker build -t 11-testcontainers-python-with-report-accessible-from-host:0.1 \
--build-arg USER_ID=$(id -u) \
--build-arg GROUP_ID=$(id -g) \
-f Dockerfile .
```

- Create new folder called `report` in `$HOME/training-devops-solutions/docker/11-testcontainers-python-with-report-accessible-from-host/`

```
mkdir $HOME/training-devops-solutions/docker/11-testcontainers-python-with-report-accessible-from-host/report
```

- Run `11-task` container with testcontainers-python

```
docker run -it --name 11-task \
--mount type=bind,source=$HOME/training-devops-solutions/docker/11-testcontainers-python-with-report-accessible-from-host/report,destination=/app/report \
--network demo-network \
11-testcontainers-python-with-report-accessible-from-host:0.1
```

- Change the directory to `$HOME/training-devops-solutions/docker/11-testcontainers-python-with-report-accessible-from-host/report` folder on the host to check the report.

```
$HOME/training-devops-solutions/docker/11-testcontainers-python-with-report-accessible-from-host/report
```

- Please execute the cleanup:

```
docker rmi -f 11-testcontainers-python-with-report-accessible-from-host:0.1
```

```
docker rm 11-task
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
