FROM node:alpine as builder
WORKDIR '/usr/src/app'
COPY package.json .
RUN npm install
COPY ./ ./
RUN npm run build
# FROM 부터 다음 FROM 까지는 builder stage 라고 부름

FROM nginx
# nginx 를 위한 base image
COPY --from=builder /usr/src/app/build /usr/share/nginx/html
# build 폴더를 nginx 에서 사용할 수 있도록 복사

# 위 과정을 통해 nginx 에서 사용할 수 있는 build 폴더를 생성하고, 이를 복사하여 사용할 수 있도록 함