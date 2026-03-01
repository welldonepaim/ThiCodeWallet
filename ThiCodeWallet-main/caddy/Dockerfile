FROM node:20-alpine AS build
WORKDIR /app

COPY spa/package*.json ./
RUN npm ci

COPY spa/ ./
RUN npm run build

FROM caddy:2.8-alpine
COPY caddy/Caddyfile /etc/caddy/Caddyfile
COPY --from=build /app/dist /srv
