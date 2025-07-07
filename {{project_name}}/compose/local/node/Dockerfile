FROM oven/bun:1
WORKDIR /app

COPY ./package.json ./bun.lock /app/

RUN bun install --no-cache

ENV PATH=./node_modules/.bin/:$PATH