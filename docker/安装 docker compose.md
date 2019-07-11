# 安装 docker compose

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

## 测试