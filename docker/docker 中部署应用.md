# docker 中部署应用

[TOC]

## 参考资料

《[Docker — 从入门到实践](https://yeasy.gitbooks.io/docker_practice/content/ )》


## 常用镜像

### 应用程序

```sh
docker pull jenkinsci/blueocean:1.17.0
docker pull mysql/mysql-server:5.7.26
docker pull gogs/gogs:0.11.91
docker pull idoop/zentao:11.4.1
docker pull nginx:1.17.0-alpine
docker pull sonatype/nexus3:3.16.2
docker pull registry:2.7.1
docker pull osixia/openldap:1.2.4
docker pull osixia/phpldapadmin:0.8.0
docker pull busybox:1.31
docker pull node:8.16-alpine
docker pull redis:5.0.5-alpine
```

### 操作系统

```sh
docker pull alpine:3.10.0
```

## 实践

### 配置 禅道 zentao

```sh
docker run --name zentao -d \
    -p 40003:80 -p 40004:3306 \
    -e ADMINER_USER="admin" \
    -e ADMINER_PASSWD="PASSWORD" \
    -e BIND_ADDRESS="false" \
    -v zentao-data:/opt/zbox/ \
    --restart=always \
    idoop/zentao:11.5.1
```

### nexus

```sh
docker run --name nexus -d \
    -p 40005:8081 \
    -v nexus-data:/nexus-data \
    -v /etc/timezone:/etc/timezone:ro \
    -v /etc/localtime:/etc/localtime:ro \
    --restart=always \
    sonatype/nexus3:3.16.2
```

### registry

```sh
docker run --name registry -d \
    -p 45000:5000 \
    -v registry-data:/var/lib/registry \
    --restart=always \
    registry:2.7.1
```

### network 试验

```sh
docker run --name network-server -it \
	-h network-server \
	--network network-test \
	alpine:3.10.0 sh
	
docker run --name network-clinet -it \
	-h network-client \
	--network network-test \
	alpine:3.10.0 sh
```

在 network-client 中可以使用 ping 测试 network-server，效果如下

```sh
/ # ping network-server
PING network-server (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.111 ms
64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.136 ms
```

### redis

[Docker安装Redis及Warning解决方法](https://www.cnblogs.com/xsjzhao/p/10882870.html)

> ```shell
> [root@localhost etc]# docker run -d -p 6378:6379 --name port-redis redis
> 25075e75cd5a387c4c807e2596a31b362efd7ee50ead5f45a18638ee96e66a66
> [root@localhost etc]# docker logs port-redis
> 1:C 17 May 2019 06:36:35.043 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
> 1:C 17 May 2019 06:36:35.043 # Redis version=5.0.4, bits=64, commit=00000000, modified=0, pid=1, just started
> 1:C 17 May 2019 06:36:35.044 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
> 1:M 17 May 2019 06:36:35.046 * Running mode=standalone, port=6379.
> 1:M 17 May 2019 06:36:35.048 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
> 1:M 17 May 2019 06:36:35.048 # Server initialized
> 1:M 17 May 2019 06:36:35.048 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
> 1:M 17 May 2019 06:36:35.048 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
> 1:M 17 May 2019 06:36:35.048 * Ready to accept connections
> ```
>
> 

