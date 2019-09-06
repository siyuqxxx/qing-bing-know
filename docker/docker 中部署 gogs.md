# docker 中部署 gogs

[TOC]

# 使用 docker run 部署 gogs

## 准备工作

数据库准备

创建表 gogs utf8 utf8-general_ci

创建用户 

```SQL
grant all privileges on gogs.* to gogs@'172.17.0.%' identified by 'PASSWORD';
```

## 部署容器

```sh
docker run --name gogs -d \
    -p 43022:22 -p 43000:3000 \
    -v gogs-data:/data \
    -v /etc/timezone:/etc/timezone:ro \
    -v /etc/localtime:/etc/localtime:ro \
    --restart=always \
    gogs/gogs:latest
```

# 使用 docker-compose 部署 gogos

[docker-compose安装gogs](https://www.520mwx.com/view/44197)

docker-compose.yml

```yml
# docker version:  18.09.0+
# docker-compose version: 1.24.1+
version: "3.7"

services:
  gogs:
    image: gogs/gogs:latest
    networks:
      - gogs-net
    ports:
      - "43022:22"
      - "43000:3000"
    volumes:
      - gogs-data:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    restart: always
  mysql-gogs:
    image: mysql/mysql-server:5.7.26
    networks:
      - gogs-net
    ports:
      - "43306:3306"
    volumes:
      - mysql-data:/var/lib/mysql
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    environment:
      - MYSQL_DATABASE=gogs
      - MYSQL_USER=gogs
      - MYSQL_PASSWORD=gogs
      - MYSQL_ROOT_PASSWORD=gogs
    restart: always
networks:
  gogs-net:
    driver: bridge
volumes:
  mysql-data:
  gogs-data:
```



# 首次登陆配置

访问地址：http://192.168.32.9:43000 填写 install 表单

### 数据库配置

| 配置项         | 值              | 备注                      |
| -------------- | --------------- | ------------------------- |
| 数据库类型     | mysql           | -                         |
| 数据库主机     | mysql-gogs:3306 | myql 容器宿主主机中的地址 |
| 数据库用户     | gogs            | -                         |
| 数据库用户密码 | gogs            | -                         |
| 数据库名称     | gogs            | -                         |

### 应用基本配置

| 配置项              | 值                          | 备注                                                         |
| ------------------- | --------------------------- | ------------------------------------------------------------ |
| 应用名称            | Gogs                        | 默认                                                         |
| 仓库根目录          | /data/git/gogs-repositories | 默认                                                         |
| 运行系统用户        | git                         | -                                                            |
| 域名                | 192.168.32.9                | 宿主主机 ip                                                  |
| SSH 端口号          | 22                          | 注意这里的端口号应该是容器内部监听的端口号，不是映射出来的端口号；用户实际上使用的时候，要使用映射出来的端口号 |
| 使用内置 SSH 服务器 | 不勾选                      | 查看 dockerfile ，已经安装了 openssh                         |
| HTTP 端口号         | 3000                        | 注意这里的端口号应该是容器内部监听的端口号，不是映射出来的端口号；用户实际上使用的时候，要使用映射出来的端口号 |
| 应用 URL            | http://192.168.32.9:43000   | 宿主主机 ip:映射出来的端口                                   |
| 日志路径            | /app/gogs/log               | 默认                                                         |

## 配置 SSH 在 web 页面上正确展示

修改配置文件 `vi /data/gogs/conf/app.ini`

```sh
# 映射出来的端口号
SSH_PORT = 43022

# 容器实际监听的端口号，默认 22
SSH_LISTEN_PORT  = 22
```

注意：配置完毕需要重启 docker 容器