# syntax=docker/dockerfile:1
FROM node:20.12.0 AS builder

WORKDIR /app

ARG VERSION=main
ARG AF_BASIC_AUTH_USER
ARG AF_BASIC_AUTH_PWD

ENV AF_BASIC_AUTH_USER=${AF_BASIC_AUTH_USER}
ENV AF_BASIC_AUTH_PWD=${AF_BASIC_AUTH_PWD}

RUN npm install -g pnpm@8.5.0
RUN git clone --depth 1 --branch ${VERSION} https://github.com/bos-hieu/AppFlowy-Web.git .
RUN pnpm install
RUN sed -i 's|https://test.appflowy.cloud||g' src/components/main/app.hooks.ts
RUN pnpm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
