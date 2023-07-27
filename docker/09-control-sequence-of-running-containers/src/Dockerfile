FROM gradle:jdk17-alpine

WORKDIR /app

COPY ./testcontainers-java ./

ENV DOCKER_HOST=tcp://dind-container:2375

WORKDIR /app/examples/redis-backed-cache-testng

CMD gradle test 