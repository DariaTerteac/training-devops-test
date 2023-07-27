FROM powerman/dockerize as dockerize

FROM ubuntu

COPY --from=dockerize /usr/local/bin/dockerize /usr/local/bin
