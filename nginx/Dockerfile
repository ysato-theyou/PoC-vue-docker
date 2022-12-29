FROM node:16.19.0-alpine3.17 as build-stage

WORKDIR /app
COPY app/vue-app/package*.json ./
RUN npm install
COPY app/vue-app/ ./
RUN npm run build

FROM nginx:1.23.3-alpine-slim as production-stage

RUN mkdir /app
COPY --from=build-stage /app/dist /app
COPY nginx.conf /etc/nginx/nginx.conf
