FROM node:10

WORKDIR /usr/src/app

COPY ./ ./

RUN npm install

# server.js소스가 변경됐을때 소스만 변경하여 빌드하고 싶을떄
# 기존엔 빌드시 종속성도 다 install 되어 비효율적
# COPY package.json ./

# RUN npm install

# COPY ./ ./

CMD ["node", "server.js"]
