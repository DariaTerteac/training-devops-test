## Task 
Interconnect in docker 2 containers: postgres client & postgres server, by hostname, not the IP address.

## Prerequisites

- Follow prerequisites steps 1-3 from [README.md](../../README.md)

## How to run

By default  docker containers are isolated from each other and cannot communicate. By using docker networks we can connect multiple containers and allow them to communicate with each other.
Adding an alias to a container makes an additional hostname that can be used by other containers on the same network for communication. So you can use container’s hostname instead of IP address which is more comfortable and human adapted.

- Create a new docker network:

```
docker network create demo-network
``` 

- Start server container and add the network with alias:

```
docker run -d --name postgres_server --network demo-network --network-alias psql-server -e POSTGRES_PASSWORD=pass123 postgres:latest
```

- Start client container and add the same network with a different alias:

```
docker run --name postgres_client --network demo-network --network-alias psql-client -it postgres:latest psql -h psql-server -U postgres
```

- Enter password you set up earlier and you are ready to execute commands in server container
- To exit container type `exit`

Please execute the clean up
```
docker rmi -f postgres
docker rm -f postgres_server
docker rm -f postgres_client
docker system prune

```
## References
1. [Docker network](https://docs.docker.com/engine/reference/commandline/network/)
2. [PostgreSQL in Docker](https://www.baeldung.com/ops/postgresql-docker-setup)
3. [https://www.sqlshack.com/getting-started-with-postgresql-on-docker/](https://www.sqlshack.com/getting-started-with-postgresql-on-docker/)
4. [Network aliases](https://docs.docker.com/engine/reference/commandline/network_connect/#alias)
