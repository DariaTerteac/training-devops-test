## Task 
Run two containers: nginxdemos/hello inside of dind and nginxdemos/hello in local docker. 
Get access to both of them from the localhost.

## Prerequisites

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

## How to run

1. Run a container based on nginxdemos/hello and get access to it from local host

```
docker run -d -p 1234:80 nginxdemos/hello
curl localhost:1234 
or http://127.0.0.1:1234 in browser

```

2. Run a nginxdemos/hello container inside dind and provide access from localhost

- Run dind container
```
docker run --privileged -d -p 1010:2375 --name 08-docker-in-docker -e DOCKER_TLS_CERTDIR="" docker:23.0.1-dind
```

- Create a new container based on the `nginxdemos/hello` image in docker-in-docker.
```
docker -H localhost:1010 run -d -p 8081:80 nginxdemos/hello
```

- Check if container was created

```
docker -H localhost:1010 ps
```

- Receive `08-docker-in-docker` container IP address.
```
docker inspect 08-docker-in-docker
```

- Make request to the nginx server running in the “nginxdemos/hello” in dind container
curl http://<container's IP address>:8081

Please execute the cleanup

```
docker rm -f $(docker ps -aq) 
docker rmi -f $(docker images -q)    
docker system prune
```

## References

1. [Get Started with Docker](https://www.docker.com/get-started/)
2. [docker images](https://docs.docker.com/engine/reference/commandline/images/)
3. [docker build](https://docs.docker.com/engine/reference/commandline/build/)
4. [docker run](https://docs.docker.com/engine/reference/commandline/run/)
5. [Docker in Docker](https://shisho.dev/blog/posts/docker-in-docker/)
6. [nginxdemos/hello](https://hub.docker.com/r/nginxdemos/hello/)