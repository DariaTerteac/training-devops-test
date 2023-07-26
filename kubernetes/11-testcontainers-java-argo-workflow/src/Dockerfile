FROM powerman/dockerize as dockerize

FROM gradle:jdk17-alpine

COPY ./testcontainers-java /app

COPY --from=dockerize /usr/local/bin/dockerize /usr/local/bin

ENV DOCKER_HOST=tcp://dind-container:2375

WORKDIR /app/examples/redis-backed-cache-testng

CMD dockerize -wait $DOCKER_HOST gradle test