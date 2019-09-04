FROM postgres:9.5-alpine
LABEL maintainer="jesse@weisner.ca"
LABEL build_id="1567622075"

ENV RUNUSER postgres
ENV HOME /postgresql
ENV PGDATA /postgresql/data

RUN apk add --no-cache \
        patch

# Add docker-entrypoint script base
ADD https://github.com/itsbcit/docker-entrypoint/releases/download/v1.5/docker-entrypoint.tar.gz /docker-entrypoint.tar.gz
RUN tar zxvf docker-entrypoint.tar.gz && rm -f docker-entrypoint.tar.gz \
 && chmod -R 555 /docker-entrypoint.* \
 && chmod 664 /etc/passwd /etc/group /etc/shadow \
 && chown 0:0 /etc/shadow \
 && chmod 775 /etc

RUN mkdir /postgresql \
 && chown 0:0 /postgresql \
 && chmod 775 /postgresql

VOLUME /postgresql

RUN deluser postgres
