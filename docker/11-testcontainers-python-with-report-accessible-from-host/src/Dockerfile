FROM powerman/dockerize as dockerize
FROM python:3.9-slim

ARG USER_ID=
ARG GROUP_ID=

WORKDIR /app

ENV USER_ID=${USER_ID}
ENV GROUP_ID=${GROUP_ID}
ENV DOCKER_HOST=tcp://dind-container:2375

COPY ./testcontainers-python/redis ./testcontainers-python/redis
COPY --from=dockerize /usr/local/bin/dockerize /usr/local/bin

WORKDIR /app/testcontainers-python/redis

RUN pip install pytest-html
RUN pip install testcontainers-redis

RUN mkdir -p /app/report

CMD dockerize -wait $DOCKER_HOST pytest -s --html=/app/report/report.html && chown -R $USER_ID:$GROUP_ID /app/report