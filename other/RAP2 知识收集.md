# RAP2 知识收集

[TOC]

# 参考资料

[github/thx/rap2-delos](https://github.com/thx/rap2-delos) - 后台

[github/thx/rap2-dolores](https://github.com/thx/rap2-dolores) - 前台

# 安装部署

## 后台安装

vi docker-compose.yml

```yml
# mail@dongguochao.com

version: '2.2'

services:
  delos:
    container_name: rap2-delos
    # build from ./Dockerfile
    # build: .
    # build from images
    # you can find last tag from https://hub.docker.com/r/blackdog1987/rap2-delos
    image: blackdog1987/rap2-delos:2.6.aa3be03
    environment:
      # if you have your own mysql, config it here, and disable the 'mysql' config blow
      - MYSQL_URL=rap2-mysql # links will maintain /etc/hosts, just use 'container_name'
      - MYSQL_PORT=3306
      - MYSQL_USERNAME=root
      - MYSQL_PASSWD=
      - MYSQL_SCHEMA=rap2

      # redis config
      - REDIS_URL=rap2-redis
      - REDIS_PORT=6379

      # production / development
      - NODE_ENV=production
    working_dir: /app
    privileged: true
    ###### 'sleep 30 && node scripts/init' will drop the tables
    ###### RUN ONLY ONCE THEN REMOVE 'sleep 30 && node scripts/init'
    command: /bin/sh -c 'sleep 30; node scripts/init; node dispatch.js'
    # init the databases
    # command: sleep 30 && node scripts/init && node dispatch.js
    # without init
    # command: node dispatch.js
    links:
      - redis
      - mysql
    depends_on:
      - redis
      - mysql
    ports:
      - "40008:8080"  # expose 38080

  redis:
    container_name: rap2-redis
    image: redis:4.0.9

  # disable this if you have your own mysql
  mysql:
    container_name: rap2-mysql
    image: mysql:5.7.22
    # expose 33306 to client (navicat)
    ports:
      - 40009:3306
    volumes:
      # change './docker/mysql/volume' to your own path
      # WARNING: without this line, your data will be lost.
      # - "./docker/mysql/volume:/var/lib/mysql"
      - rap2-mysql-data:/var/lib/mysql
    command: mysqld --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --init-connect='SET NAMES utf8mb4;' --innodb-flush-log-at-trx-commit=0
    environment:
      MYSQL_ALLOW_EMPTY_PASSWORD: "true"
      MYSQL_DATABASE: "rap2"
      MYSQL_USER: "root"
      MYSQL_PASSWORD: ""
  dolores:
    container_name: rap2-dolores
    image: qsy/rap2-dolores:2.0
    environment:
      - NODE_ENV=development
    privileged: true
    command: /bin/sh -c 'npm run dev'
    links:
      - delos
    depends_on:
      - delos
    ports:
      - "40010:80"
      - "40011:3000"
volumes:
  rap2-mysql-data:
```

## 前台安装

vi dockerfile

### alpine

```dockerfile
FROM node:8.16-alpine

ENV RAP2_DOLORES 2.1.3
WORKDIR /opt/rap2-dolores
EXPOSE 80

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories \
  && apk --update-cache update && apk add curl python2 make g++ \
  && curl -OL https://github.com/thx/rap2-dolores/archive/2.1.3.tar.gz \
  && tar zxf 2.1.3.tar.gz \
  && mv rap2-dolores-2.1.3/* . && rm -rf 2.1.3.tar.gz rap2-dolores-2.1.3/ \
  && sed -i 's/rap2api.taobao.org/10.20.1.51:40008/' src/config/config.prod.ts \
  && sed -i 's/127.0.0.1:8080/10.20.1.51:40008/' src/config/config.dev.ts \
  && npm install -g --unsafe-perm serve node-sass --registry=http://registry.npm.taobao.org \
  && npm install && npm run build

CMD ["serve", "-s", "./build", "-p", "80"]
```



### debian

```dockerfile
FROM node:8.16-stretch

ENV RAP2_DOLORES   2.1.3
WORKDIR /opt/rap2-dolores
EXPOSE 80

RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/' /etc/apt/sources.list \
  && apt update && apt upgrade -y && apt install xsel -y \
  && curl -OL https://github.com/thx/rap2-dolores/archive/2.1.3.tar.gz \
  && tar zxf 2.1.3.tar.gz \
  && mv rap2-dolores-2.1.3/* . && rm -rf 2.1.3.tar.gz rap2-dolores-2.1.3/ \
  && sed -i 's/rap2api.taobao.org/rap2-delos:8080/' src/config/config.prod.ts \
  && sed -i 's/127.0.0.1/rap2-delos/' src/config/config.dev.ts \
  && npm install -g --unsafe-perm serve node-sass \
  && npm install && npm run build

ENTRYPOINT ["serve", "-s", "./build", "-p", "80"]
```

sudo docker build -t="qsy/rap2-dolores:2.0" .

```sh
docker run --name dolores -it \
  -p 40011:80 \
  -p 40012:3000 \
  --network rap2_default \
  qsy/rap2-dolores sh
```

```sh
FROM qsy/rap2-dolores:1.0

ENTRYPOINT ["serve -s ./build -p 80"]
```

```sh
docker run --name temp -d \
  -p 40013:80 \
  -p 40014:3000 \
  --network rap2_default \
  qsy/rap2-dolores:1.1
```

[接口文档管理神器RAP2安装和部署](https://www.cnblogs.com/operationhome/p/10038469.html)

> ```sh
> # 运行 
> serve -s ./build -p 80
> -p 为指定端口
> # 后台运行
> nohup  serve -s ./build -p 80  &
> ```

[Docker搭建Rap2](https://www.liangzl.com/get-article-detail-125295.html)