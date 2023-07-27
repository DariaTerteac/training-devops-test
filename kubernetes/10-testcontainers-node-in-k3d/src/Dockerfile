FROM powerman/dockerize:0.19.0 as dockerize
FROM node:slim

ARG USER_ID=1000
ARG GROUP_ID=1000

ENV USER_ID=${USER_ID}
ENV GROUP_ID=${GROUP_ID}

WORKDIR /app/

COPY --from=dockerize /usr/local/bin/dockerize /usr/local/bin
COPY . .

WORKDIR /app/testcontainers-node
RUN npm install --save-dev jest-html-reporter

WORKDIR /app/
RUN cp /app/jest.config.js /app/testcontainers-node/

ENV DOCKER_HOST=tcp://dind-container:2375
WORKDIR /app/testcontainers-node/src/modules/postgresql

RUN npm install
RUN mkdir /app/jest-test-report
CMD dockerize -wait $DOCKER_HOST -timeout 30s npm test postgresql-container.test.ts && chown -R $USER_ID:$GROUP_ID /app/testcontainers-node/jest-test-report
