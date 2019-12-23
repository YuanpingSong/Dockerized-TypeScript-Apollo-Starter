# Development
FROM node:alpine as development

RUN apk update && \
    apk upgrade && \
    apk --no-cache add npm git 

WORKDIR /home/node/app

COPY . .

RUN npm install && \
    npm run build-ts

CMD ["npm", "run", "watch"]


# Builder, Using the multi-stage build pattern:
# https://docs.docker.com/develop/develop-images/multistage-build/
FROM node:alpine as builder

RUN apk update && \
    apk upgrade && \
    apk --no-cache add npm git 

WORKDIR /home/node/app

COPY . .

RUN npm install --production


# Production builder
FROM node:alpine as production 

WORKDIR /home/node/app

COPY --from=development /home/node/app/dist ./dist
COPY --from=builder /home/node/app/node_modules ./node_modules

CMD node dist/server.js