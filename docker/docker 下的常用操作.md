# docker 下的常用操作

[TOC]

## 使用 bash 操作正在运行的 容器

```sh
docker exec -it test /bin/bash
```

## docker 下检查远程主机端口是否开放

```sh
nc -v host port
```

## 查看 docker 容器 日志 以 tomcat 为例

docker logs 命令描述，参考：[docker logs－查看docker容器日志][4.1]


```sh
docker logs -ft --tail=100 test-tomcat
```

[Docker容器进入的4种方式以及tomcat查看日志][4.2] 这篇文章介绍了 4 中访问日志的方式

现在，有一个问题，linux 中有各种各样的日志，容器怎么知道 docker logs 命令是想要查看什么日志的？

[4.1]:(https://www.cnblogs.com/gylhaut/p/9317843.html)
[4.2]:(http://www.cnblogs.com/gyadmin/p/7814256.html)

## 如何查看容器挂载的目录

docker inspect - mounts 参考： [关于Docker目录挂载的总结][5.1]

[5.1]:https://www.cnblogs.com/ivictor/p/4834864.html

挂载了一个目录的 tomcat 容器：docker run --name test-tomcat -v /docker-share/webapps/panoramicLibrary:/usr/local/tomcat/webapps/panoramicLibrary -dP tomcat:8.5.39-jre8-slim

-v [宿主主机路径]:[容器路径]

## 修改运行中的docker容器的启动参数

[docker container update](https://docs.docker.com/engine/reference/commandline/container_update/) 官方文档

```sh
# 设置为启动 docker 自动启动容器
docker container update --restart=always 容器名字
# 设置为自动 docker 不自动启动容器
docker container update --restart=no 容器名字
```