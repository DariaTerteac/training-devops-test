FROM powerman/dockerize:0.19.0 as dockerize
FROM python:3.9.17-alpine3.18

ARG U_ID=1000
ARG G_ID=1000

ENV USER_ID=${U_ID}
ENV GROUP_ID=${G_ID}

ENV DOCKER_HOST=tcp://dind-container:2375
WORKDIR /app/

COPY --from=dockerize /usr/local/bin/dockerize /usr/local/bin
COPY . .

RUN pip install --upgrade pip && pip install pytest

WORKDIR /app/testcontainers-python/redis

RUN pip install pytest-html

RUN pip install testcontainers-redis

RUN mkdir /app/report

CMD dockerize -wait $DOCKER_HOST -timeout 30s pytest --html=/app/report/report.html && chown -R $USER_ID:$GROUP_ID /app/report
