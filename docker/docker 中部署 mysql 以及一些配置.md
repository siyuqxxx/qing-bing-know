# docker 中部署 mysql 以及一些配置

[TOC]

# 参考资料

## 官方文档：

[2.8 Deploying MySQL on Linux with Docker](https://dev.mysql.com/doc/mysql-linuxunix-excerpt/5.6/en/linux-installation-docker.html) - 概述，实际上没啥价值
[2.8.1 Basic Steps for MySQL Server Deployment with Docker](https://dev.mysql.com/doc/mysql-linuxunix-excerpt/5.6/en/docker-mysql-getting-started.html) - 包含一个简单的基本介绍，没有介绍数据持久化的问题
[2.8.2 More Topics on Deploying MySQL Server with Docker](https://dev.mysql.com/doc/mysql-linuxunix-excerpt/5.6/en/docker-mysql-more-topics.html) - 介绍了需要的参数，包括持久化问题

# 部署 mysql

我采用的命令是: 

```sh
docker run --name mysql -d \
    -p 3306:3306 \
    -v mysql-data:/var/lib/mysql \
    -v /etc/timezone:/etc/timezone \
    -v /etc/localtime:/etc/localtime \
    --restart=always \
    mysql/mysql-server:5.7.26
```

配置完 mysql 以后，设置新密码

首先，登录 mysql

`docker exec -it mysql mysql -uroot -pxxxxxx`

修改密码

```sh
alter user `root`@`localhost` identified by 'PASSWORD';
```

[MySQL5.7 添加用户、删除用户与授权](https://www.cnblogs.com/xujishou/p/6306765.html)

```sh
# 创建用户同时授权
grant all privileges on *.* to test@localhost identified by 'PASSWORD';
```
这里可以用宿主机的 ip 地址可以作为局域网访问的授权 host，比如 

```sh
# 创建用户同时授权
grant all privileges on *.* to test@'192.168.0.%' identified by 'PASSWORD';
# 在 linux 下安装 docker, 并在 linux 下使用
grant all privileges on *.* to test@'172.17.0.%' identified by 'PASSWORD';
```

[各个平台的mysql重启命令](https://www.cnblogs.com/adolfmc/p/5497974.html)

## 开启 回滚

```sh
echo "
# 开启 binlog 支持数据回滚
server_id=1
binlog_format=row
binlog_row_image=full
# 控制 binlog 文件大小
log_bin=/var/log/mysql/mysql-bin.log
max_binlog_size=1G
expire_logs_days=30
">>/etc/my.cnf
```

注意给权限，否则将导致无法启动问题，参见：[mysql配置binlog后无法启动解决办法](https://blog.csdn.net/LZMLZMLZM268/article/details/87916675)

```sh
mkdir -p /var/log/mysql
chown -R mysql:mysql /var/log/mysql
```

## 开启 慢日志

```sh
echo "
# 开启慢日志
slow_query_log = ON
slow_query_log_file = /var/log/mysql/mysql-slow.log
long_query_time = 1
">>/etc/my.cnf
```

## innodb 独占表空间

innodb_file_per_table
```sh
echo "
# innodb 独占表空间
innodb_file_per_table = 1
">>/etc/my.cnf
```

参考资料:

 [MySQL Server参数优化 - innodb_file_per_table（独立表空间）](https://blog.csdn.net/jesseyoung/article/details/42236615)

## mysql5.7支持group by
```sh
echo "
# mysql5.7支持group by
sql_mode = STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
">>/etc/my.cnf
```

参考资料：

[mysql报错this is incompatible with sql_mode=only_full_group_by](http://www.mamicode.com/info-detail-2560516.html)

[MySQL 5.7默认ONLY_FULL_GROUP_BY语义介绍](https://www.cnblogs.com/xzjf/p/8466858.html)

## mysql 初始化时，执行初始化脚本

[让docker中的mysql启动时自动执行sql](https://blog.csdn.net/boling_cavalry/article/details/71055159)

[Docker Hub MySQL官方镜像实现首次启动后初始化库表](https://yov.oschina.io/article/容器/Docker/Docker Hub mysql官方镜像实现首次启动后初始化库表/)