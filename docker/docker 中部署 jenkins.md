# docker 中部署 jenkins

[TOC]

# 时区问题

确认一下这几个文件是否存在：
/etc/timezone
/etc/localtime

如果不存在，生成的方式如下：

```sh
echo 'Asia/Shanghai' > /etc/timezone
rm -rf /etc/localtime && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime  // 如果时区不正确，需要使用 rm； 如果不存在，直接 cp
```

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
    user: root
    ports:
      - "40001:8080"
      - "40002:5000"
    volumes:
      - jenkins-data:/var/jenkins_home
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock
      - /usr/local/bin/docker:/bin/docker:ro
    restart: "no"
volumes:
  jenkins-data:

```

[Compose file version 3 reference](https://docs.docker.com/compose/compose-file/) - docker-compose 官方说明文档

> docker compose file format version 3.7  <---- match ----> Docker Engine release  version 18.06.0+

参考资料：

[Docker版本Jenkins的使用](https://www.jianshu.com/p/0391e225e4a6)

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

# 部署在 docker 中的 jenkins 使用宿主机的 docker

[使用Jenkins MultiBranch实现Docker build镜像并push到DockerHub](https://blog.csdn.net/haiyanggeng/article/details/85344103)

> docker run 参数
>
> ```sh
> docker run --name jenkins -d \
>   -p 8080:8080 -p 50000:50000 \
>   -v ~/workspace/jenkins_home:/var/jenkins_home \
>   -v /var/run/docker.sock:/var/run/docker.sock \
>   -v /usr/local/bin/docker:/bin/docker \
>   -t jenkinsci/jenkins:2.150.1
> ```
>
> 使用 root 登陆 jenkins 容器，并赋予执行权限（此方法，宿主机重启后失效）
>
> ```sh
> docker exec -it -u root jenkins sh
> chmod +x /var/run/docker.sock
> chown jenkins:jenkins /var/run/docker.sock
> ```

[问题解决：用Docker启动Jenkins出现权限问题](https://blog.csdn.net/qq_36792209/article/details/82695750)

> ##### 在docker-compose.yml文件中加入user

# jenkins pileline 使用 npm 构建前端项目

[jenkins使用pipeline构建nodejs应用](https://blog.csdn.net/qq_35299863/article/details/84240379)

# jenkinsfile 转义字符

[[Jenkins][JenkinsFile][Linux] sh 替换属性文件properties的内容](https://blog.csdn.net/weixin_42713970/article/details/86511642)

> 多行，转义 `\\`
>
> ```jenkinsfile
> pipeline {
>     agent any
>     stages {
>        stage('configEnv') {
>          steps {
>            sh '''
>              sed -i "s#^user.name=.*#user.name=用户名#g"  path/demo.properties
>              sed -i "s#^user.password=.*#user.password=密码#g"  path/demo.properties
>            '''
>          }
>        }
>      }
>   }
> ```

# pipeline 用户输入

[Jenkins Pipeline语法（上）](https://www.jianshu.com/p/18327865a38a)

[Jenkins Pipeline语法（中）](https://www.jianshu.com/p/7a852d58d9a9)

[Jenkins Pipeline语法（下）](https://www.jianshu.com/p/797e27209761)

```jenkinsfile
pipeline {
    agent any
    stages {
        stage('Example') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }
    }
}
```

# pipeline 用户选择

```jenkinsfile
pipeline {
    agent any
    parameters {
        choice(name: 'CHOICE', choices: ['local', 'dev', 'test', 'prod'], description: 'Pick something')
    }
    stages {
        stage('Example') {
            when {
                environment name: 'CHOICE', value: 'local'
            }
            steps {
                echo "local, hello world!"
            }
        }
        stage('Example2') {
            when {
                environment name: 'CHOICE', value: 'dev'
            }
            steps {
                echo "dev, hello world!"
            }
        }
    }
}
```

# pipeline 用户多选

[jenkins参数化构建过程（添加多选框）](https://blog.csdn.net/e295166319/article/details/54017231)

[如何在Jenkins文件中传递多选值参数（Groovy）](https://www.soinside.com/question/YkQkr9EYH7UbXBZcrGaZBg)

# 流水线示例

[采用jenkins pipeline实现自动构建并部署至k8s](https://www.jianshu.com/p/2d89fd1b4403)