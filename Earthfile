VERSION 0.7

build: 
  FROM node:16.10.0-alpine
  WORKDIR /app
  COPY package.json ./
  COPY yarn.lock ./
  RUN yarn install --frozen-lockfile
  COPY . .
  RUN yarn build
  SAVE ARTIFACT ./build prod

docker:
  FROM nginx:stable-alpine 
  COPY +build/prod /usr/share/nginx/html
  SAVE IMAGE --push harbor.axinode.com:7443/axilog/examples:react
  
run:
  FROM earthly/dind:alpine
  WITH DOCKER --load app:test=+docker
    RUN docker run --rm -p 3000:80 app:test
  END