FROM postgres:10
LABEL maintainer="jesse_weisner@bcit.ca"

ENV RUNUSER postgres
ENV HOME /postgresql
ENV PGDATA /postgresql/data

# Add patch
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    patch \
 && rm -rf /var/lib/apt/lists/*

# Add docker-entrypoint script base
ENV DE_VERSION v1.1
ADD https://github.com/itsbcit/docker-entrypoint/releases/download/${DE_VERSION}/docker-entrypoint.tar.gz /docker-entrypoint.tar.gz
RUN tar zxvf docker-entrypoint.tar.gz docker-entrypoint.d && rm -f docker-entrypoint.tar.gz

# Apply docker-entrypoint.d patch
COPY docker-entrypoint.sh.patch /tmp/docker-entrypoint.sh.patch
RUN patch -p0 /usr/local/bin/docker-entrypoint.sh < /tmp/docker-entrypoint.sh.patch \
 && rm -f /tmp/docker-entrypoint.sh.patch

# Allow resolve-userid.sh script to run
RUN chmod 664 /etc/passwd /etc/group

RUN mkdir /postgresql \
 && chown 0:0 /postgresql \
 && chmod 775 /postgresql

VOLUME /postgresql

RUN userdel postgres
