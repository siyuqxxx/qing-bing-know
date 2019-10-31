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
      - data:/var/jenkins_home
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock
      - /usr/local/bin/docker:/bin/docker:ro
    restart: always
volumes:
  data:

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

> 在docker-compose.yml文件中加入user

## centos7 下无法使用宿主机 docker

仅针对：jenkins/jenkins:2.190.1-centos 使用这个镜像并不能使用 docker 

[在容器中操作宿主机的Docker](https://blog.csdn.net/Loiterer_Y/article/details/88126582)

> 这篇博客并不能解决问题，但是提供了一个思路，估计是缺少什么依赖造成的

# 部署在 docker 中的 jenkins 使用宿主机的 docker-compose

[docker部署Jenkins，以及在Jenkins中使用宿主机的docker/docker-compose命令](http://www.mamicode.com/info-detail-2160456.html)

> 文章可读性太差；内容好着呢

[Can not install docker-compose in docker container](https://github.com/docker/compose/issues/6004)

> You need glibc for `docker-compose` to run on alpine
>
> jenkins-blueocean 的镜像是基于 alpine 制作的，没有 glibc

[Alpine与glibc](https://www.szyhf.org/2016/12/16/alpine与glibc/)

> 1. alpine使用的glic不是一般CentOs或者Ubuntu使用的版本，某种意义上可以看作是一个简化版，它能支持大部分的应用，但bitcoind除外。
> 2. 所以使用的时候需要安装针对alpine专用的glic。
> 3. 官网建议使用busybox的方案，暂时未验证。
> 4. 使用的是另一个开源项目：https://github.com/sgerrand/alpine-pkg-glibc
> 5. alpine-pkg-glic的说明文档其实有提到，如果需要更多的locadef功能，就得再装一个glibc-bin（印象中跟报的错有关），所以解决方案就是在该项目下载另一个apk装了，解决。

[Running glibc programs](https://wiki.alpinelinux.org/wiki/Running_glibc_programs)

> 官方 wiki



由于 jenkins/blueocean 镜像基于 alpine，因此需要使用非 alpine 的镜像 jenkins/jenkins:2.190.1-centos

> 经过实践证明，使用这个镜像，docker-compose 可以使用了，但是 docker 却不能使用了



最终，采用安装 glibc 组件实现功能

```sh
# 提高 alpine 组件库速度；不是必须的
# 西安
sed -i 's/dl-cdn.alpinelinux.org/mirrors.nju.edu.cn/g' /etc/apk/repositories
# 北京
sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories

wget -c https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.30-r0/glibc-2.30-r0.apk
wget -c https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.30-r0/glibc-bin-2.30-r0.apk

apk --update-cache update
wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub
apk add bash glibc-2.30-r0.apk glibc-bin-2.30-r0.apk
rm -rf glibc-2.30-r0.apk glibc-bin-2.30-r0.apk
```

注意，由于 github 下载比较慢，要注意观察，不行就杀死 wget 重新运行



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

[<三十二>Jenkins-pipeline学习笔记–Jenkinsfile声明式语法详解(二)](http://www.eryajf.net/3298.html)

# 流水线示例

[采用jenkins pipeline实现自动构建并部署至k8s](https://www.jianshu.com/p/2d89fd1b4403)

[基于Jenkins Pipeline的ASP.NET Core持续集成实践](https://www.cnblogs.com/lonelyxmas/p/10713783.html)

> 这篇文章，看起来高大上

# 手动安装 nodejs

**注意**：这里特指，在 docker alpine 镜像下部署 jenkins

[Official Alpine Linux mirrors](https://mirrors.alpinelinux.org/)

> 检索国内镜像，选择速度比较快的即可

```sh
# 提高 alpine 组件库速度；不是必须的
# 西安
sed -i 's/dl-cdn.alpinelinux.org/mirrors.nju.edu.cn/g' /etc/apk/repositories
# 北京
sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories

apk --update-cache update && apk add nodejs npm
node -v
npm -v
```

centos

[github/nodesource/distributions](https://github.com/nodesource/distributions)

[CentOS7利用yum安装node.js](https://www.cnblogs.com/yanwanglol/p/8762488.html)

```sh
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
yum install -y nodejs
node -v
npm -v
```



# 手动安装 maven

**注意**：这里特指，在 docker alpine 镜像下部署 jenkins

```sh
mkdir /var/jenkins_home/tools
cd /var/jenkins_home/tools/
curl -O https://archive.apache.org/dist/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz
tar -zxf apache-maven-3.6.1-bin.tar.gz && rm -f apache-maven-3.6.1-bin.tar.gz
cd apache-maven-3.6.1/bin
. mvn
cp ../conf/settings.xml ~/.m2/ && vi ~/.m2/settings.xml
```

阿里云镜像

```xml
    <mirror>
      <id>alimaven</id>
      <mirrorOf>central</mirrorOf>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
    </mirror>
```



# pipeline 中使用 tag 构建项目

[When using tags in Jenkins Pipeline](https://jenkins.io/blog/2018/05/16/pipelines-with-git-tags/)

> ![Configuring the Multibranch Pipeline](https://jenkins.io/images/post-images/pipeline-tags/branch-source.png)