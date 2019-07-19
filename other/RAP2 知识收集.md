# RAP2 知识收集

[TOC]

# 参考资料

[github/thx/rap2-delos](https://github.com/thx/rap2-delos) - 后台

[github/thx/rap2-dolores](https://github.com/thx/rap2-dolores) - 前台

# 安装部署

后台安装

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
    #ports:
    #   - 33306:3306
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
volumes:
  rap2-mysql-data:
```

前台安装

vi dockerfile

```dockerfile
FROM node:8.16-alpine

WORKDIR /opt/rap2-dolores
EXPOSE 80

COPY rap2-dolores-2.1.3 ./


RUN sed -i 's/rap2api\.taobao\.org/rap2-delos/' src/config/config.prod.ts && \
        npm run build && \
        serve -s ./build -p 80

CMD [ "sh" ]
```

```dockerfile
FROM node:8.16-alpine

WORKDIR /opt/rap2-dolores
EXPOSE 80

COPY rap2-dolores-2.1.3 ./


RUN sed -i 's/rap2api\.taobao\.org/rap2-delos/' src/config/config.prod.ts && \
        npm run build && \
        serve -s ./build -p 80

CMD [ "sh" ]
```

sudo docker build -t="qsy/rap2-dolors" .

```
docker run --name dolors -it \
	--network rap2_default \
    --rm \
	qsy/rap2-dolors sh
```

