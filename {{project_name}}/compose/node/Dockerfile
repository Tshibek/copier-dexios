FROM oven/bun:1
WORKDIR /app

COPY ./package.json ./bun.lock /app/

RUN bun install && bun pm cache rm -g -f

ENV PATH=./node_modules/.bin/:$PATH