FROM node:20-alpine
WORKDIR /api

COPY . .
RUN rm -rf node_modules
RUN npm i 

CMD [ "node","src/index.js" ]
EXPOSE 5002
