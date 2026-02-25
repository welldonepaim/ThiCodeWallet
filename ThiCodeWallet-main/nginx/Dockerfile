FROM node:20-alpine AS build
WORKDIR /app

COPY spa/package*.json ./
RUN npm ci

COPY spa/ ./
RUN npm run build

FROM nginx:1.25-alpine
RUN rm -f /etc/nginx/conf.d/default.conf
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /var/www/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
