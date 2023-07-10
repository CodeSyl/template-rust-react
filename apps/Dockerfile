FROM node:18-alpine AS base

RUN npm i -g pnpm

FROM base AS dependencies

ARG APP_DIR=web

WORKDIR /app

COPY ./${APP_DIR}/package.json pnpm-lock.yaml ./

RUN pnpm install

FROM base AS build
ARG APP_DIR=web

WORKDIR /app

COPY ./${APP_DIR} .

COPY --from=dependencies /app/node_modules ./node_modules

RUN pnpm build

FROM build AS serve

WORKDIR /app

COPY --from=build /app /app

CMD ["node_modules/serve/build/main.js","-s" ,"dist","-p" ,"3000"]
