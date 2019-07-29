# docker 中部署 jenkins

[TOC]

# 部署 jenkins

官方操作说明 [安装Jenkins](https://jenkins.io/zh/doc/book/installing/ )

我基于 jenkins 官方文档中的说明来部署，我采用的命令是： 

```sh
docker run --name jenkins -d \
    -p 40001:8080 -p 40002:5000 \
    -v jenkins-data:/var/jenkins_home \
    -v /etc/timezone:/etc/timezone:ro \
    -v /etc/localtime:/etc/localtime:ro \
    --restart=always \
    jenkinsci/blueocean:1.17.0
```

jenkinsci docker file github [jenkinsci/docker](https://github.com/jenkinsci/docker)

使用 docker-compose 部署

```yml
# docker version:  18.09.0+
# docker-compose version: 1.24.1+
version: "3.7"
services:
  jenkins:
    image: jenkinsci/blueocean:1.17.0
    container_name: jenkins
    hostname: jenkins
    ports:
      - "40001:8080"
      - "40002:5000"
    volumes:
      - jenkins-data:/var/jenkins_home
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    restart: "no"
volumes:
  jenkins-data:
```

[Compose file version 3 reference](https://docs.docker.com/compose/compose-file/) - docker-compose 官方说明文档

> docker compose file format version 3.7  <---- match ----> Docker Engine release  version 18.06.0+

参考资料：

[Docker版本Jenkins的使用](https://www.jianshu.com/p/0391e225e4a6)

# 时区问题

确认一下这几个文件是否存在：
/etc/timezone
/etc/localtime

如果不存在，生成的方式如下：

```sh
echo 'Asia/Shanghai' > /etc/timezone
rm -rf /etc/localtime && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime  // 如果时区不正确，需要使用 rm； 如果不存在，直接 cp
```

# 配置 git 私钥 和 公钥

采用 blue ocean 可以不配置公钥和私钥

# idea 配置 jenkins plugin

[IntelliJ配置jenkins服务的Crumb Data](https://blog.csdn.net/shy2shy/article/details/78525543)

# 该Jenkins实例似乎已离线

[解决：安装Jenkins时web界面出现该jenkins实例似乎已离线](https://www.cnblogs.com/forever521Lee/p/9356212.html)

[安装Jenkins时web界面出现该jenkins实例似乎已离线](https://blog.csdn.net/vicky_lov/article/details/83382463)

> 这个是 docker 下安装 jenkins 时，hudson.model.UpdateCenter.xml 的位置
>
> 在jenkins_home地址中，找到 /hudson.model.UpdateCenter.xml

```sh
vi /var/jenkins_home/hudson.model.UpdateCenter.xml

# 添加一个 site
  <site>
    <id>qinghua</id>
    <url>https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json</url>
  </site>
```

