
FROM docker.io/postgres:17

WORKDIR /usr/local/bin

COPY ./compose/production/postgres/maintenance ./maintenance/

RUN set -ex \
    && chmod +x ./maintenance/* \
    && mv ./maintenance/* ./ \
    && rmdir ./maintenance