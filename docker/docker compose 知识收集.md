# docker compose 知识收集

[TOC]

## 安装

github下载 实在是太慢了，通过这篇博客解决 [国内高速下载Docker 以及 docker-compose 地址](https://blog.csdn.net/kozazyh/article/details/79764746)，摘录如下：

```sh
# 下载 docker-compose
curl -L https://get.daocloud.io/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
# 验证
docker-compose --version
```

## docker-compose.yml

services.{service_name}.restart 可用参数

[docker-compose restart inverval](https://stackoverflow.com/questions/43039922/docker-compose-restart-inverval)

> - `always`
> - `on-failure`
>   - The `on-failure` policy is a bit interesting as it allows you to tell Docker to restart a container if the exit code indicates error but not if the exit code indicates success. You can also specify a maximum number of times Docker will automatically restart the container. like on-failure:3 It will retry 3 times. 
>   - `on-failure` 策略有点有趣，因为如果退出代码指示错误，它允许您告诉Docker重新启动容器，但如果退出代码指示成功则不允许。 您还可以指定Docker自动重启容器的最大次数。比如 `on-failure:3`：3它将重试3次。
> - `unless-stopped`
>   - The `unless-stopped` restart policy behaves the same as always with one exception. When a container is stopped and the server is rebooted or the Docker service is restarted, the container will not be restarted. 
>   - `unless-stopped`重启策略的行为与以往一样，只有一个例外。 当容器停止并重新启动服务器或重新启动Docker服务时，将不会重新启动容器。

[Compose file version 3 reference](https://docs.docker.com/compose/compose-file/) - 官方文档

> ```
> restart: "no"
> restart: always
> restart: on-failure
> restart: unless-stopped
> ```