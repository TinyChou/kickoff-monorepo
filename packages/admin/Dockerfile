FROM node:16.15.0-bullseye-slim AS build
ARG BUILD_ID
ENV BUILD_ID=$BUILD_ID
LABEL stage=builder
LABEL build=$BUILD_ID
WORKDIR /usr/src/app
COPY . /usr/src/app
RUN npm i -g pnpm@8
RUN pnpm i && pnpm build

FROM nginx
LABEL stage=runner
LABEL build=$BUILD_ID
COPY --from=build /usr/src/app/dist /usr/share/nginx/html
COPY --from=build /usr/src/app/docker/nginx/conf.d /etc/nginx/conf.d
COPY --from=build /usr/src/app/docker/nginx/nginx.conf /etc/nginx/nginx.conf