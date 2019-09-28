# docker 的常用配置

[TOC]

## 参考资料

[如何清理Docker占用的磁盘空间?](https://blog.csdn.net/B9Q8e64lO6mm/article/details/79070442)
[Docker磁盘空间使用分析与清理](https://www.jianshu.com/p/7aeafe2ea792)
[docker资源分配篇](https://blog.51cto.com/lidefu/2369147)

# 安装

[Get Docker Engine - Community for CentOS](https://docs.docker.com/install/linux/docker-ce/centos/) - 官网

```sh
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
  
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install -y docker-ce docker-ce-cli containerd.io

sudo systemctl enable docker
sudo systemctl start docker
```

## docker pull 慢

`docker-machine ssh` 连接宿主机 default 

```sh
sudo vi /etc/docker/daemon.json

{
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/", "http://hub-mirror.c.163.com"]
}

sudo /etc/init.d/docker restart
```

或者

```sh
echo "{
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/", "http://hub-mirror.c.163.com"]
}">/etc/docker/daemon.json

sudo /etc/init.d/docker restart
```

注意：
+ 这个更改会同时改变 Kitematic 的镜像源
+ 有时候，daemon.json 不存在，新增一个就行

[Docker pull卡住，解决方案][Docker pull卡住，解决方案] 这篇文章中提到几个可以用的国内加速器（中国科技大学的镜像加速器、阿里云加速器、DaoCloud 加速器）


中国科技大学的镜像加速器，进一步的信息可以访问：[Docker Hub 源使用帮助](http://mirrors.ustc.edu.cn/help/dockerhub.html?highlight=docker)

[docker hub下载速度太慢，更新国内源](https://www.jianshu.com/p/87d5799211d9) 另外一个国内源 http://hub-mirror.c.163.com

[docker镜像源加速设置之windows][pull 慢] 这篇文章讲解了 ToolBox 下，如何设置

官方文档 [Registry as a pull through cache](https://docs.docker.com/registry/recipes/mirror/) 注意，docker 18 版 以后，修改 daemon 的命令有调整

```sh
# 原来
docker --registry-mirror=https://registry.docker-cn.com daemon
# 现在
dockerd --registry-mirror=https://registry.docker-cn.com
```

操作 daemon 参考这篇文章，[Docker学习笔记 — 配置国内免费registry mirror](https://blog.csdn.net/qq_26091271/article/details/51501768)

[pull 慢]:https://www.jianshu.com/p/522e7dacc55e

[Docker pull卡住，解决方案]:https://www.jianshu.com/p/a024dc5ade92

## docker 容器中部署应用存在 时差 8 小时 问题

一开始发现时差问题时，各种解决不了，指导阅读到了这篇文章 [boot2docker持久化配置，修改时区时间和docker国内镜像地址][toolbox 时区持久化] ，才知道每一次重启 宿主机 default 的时候，都是重新加载的，因此，必须有一个持久化的方法。但是，这篇文章说明的不是很详细，直到看到了这篇文章 [boot2docker 遇到的几个坑][boot2docker 遇到的几个坑] 觉得特别赞，按照这个方法成功的解决了时差 8 小时的问题；

主要问题一共有这么几个：

+ 宿主机 default 上并没有 /user/share/zoneinfo 这个目录，因此网上的好多文章并不能用
+ 时差配置完毕以后，重启电脑，或者重启宿主机都会导致时差配置失效，不能持久化

因此，解决方案如下：

+ 从包含正确时区的 linux 设备上 使用远程命令 scp 拷贝一个正确的 localtime 文件到/mnt/sda1下；
  + 命令： `sudo scp root@192.168.3.232:/etc/localtime /mnt/sda1/`
+ 然后在 /mnt/sda1/var/lib/boot2doceker 下新建 bootlocal.sh文件，并赋予执行权限；
  + 命令 `sudo echo 'cp -f /mnt/sda1/localtime /etc/localtime && echo "Asia/Shanghai" > /etc/timezone;'>/var/lib/boot2docker/bootlocal.sh`
  + 权限 `chmod +x /var/lib/boot2docker/bootlocal.sh`

[boot2docker 遇到的几个坑]:http://blog.sina.com.cn/s/blog_67fbe4650102xgmc.html

[toolbox 时区持久化]:https://blog.csdn.net/dwz0121/article/details/86470332

## dokcer 容器被局域网发现

[docker toolbox无法被外网访问解决方法](https://blog.csdn.net/a462464126/article/details/83061848) 看完这篇文章，突然觉得好简单，绕了天大的一个圈子，然后转了回来。好了现在解决了，开心！

[Docker高级网络实践之 玩转Linux network namespace & pipework](https://blog.51cto.com/ganbing/2088899)

上面这篇文章，有很多关于 linux ip 这个命令的使用，关于 ip 命令的详解见文章：[Linux ip命令详解](https://blog.csdn.net/haoshuwei531024/article/details/47952629)


[IP 命令使用方法](https://www.jianshu.com/p/346569b7b899) 包含 ip link set 命令组的介绍，摘录如下

```sh
ip -s -s link show                                  # 显示所有接口详细信息
ip -s -s link show eth1.11                          # 显示单独接口信息
ip link set dev eth1 up                             # 启动设备，相当于 ifconfig eth1 up
ip link set dev eth1 down                           # 停止设备，相当于 ifconfig eth1 down
ip link set dev eth1 txqueuelen 100                 # 改变设备传输队列长度
ip link set dev eth1 mtu 1200                       # 改变 MTU 长度
ip link set dev eth1 address 00:00:00:AA:BB:CC      # 改变 MAC 地址
ip link set dev eth1 name myeth                     # 接口名变更
```

[Linux网络命名空间](https://www.jianshu.com/p/369e50201bce) 这篇文章觉得特别好，其中介绍到：虚拟网卡 veth，是成对出现的，就像一个管道的两端，从这个管道的一端的veth进去的数据会从另一端的veth再出来。因此，创建虚拟网卡 veth 都是成对出现

上面这篇文章中关联了几篇文章，觉得有价值的如下：

[DOCKER基础技术：LINUX NAMESPACE（上）](https://coolshell.cn/articles/17010.html)
[DOCKER基础技术：LINUX NAMESPACE（下）](https://coolshell.cn/articles/17029.html)
[docker 网络](https://segmentfault.com/a/1190000005794036)
[Network namespaces](https://blogs.igalia.com/dpino/2016/04/10/network-namespaces/) 这里面讲了一些等效用法，有启发

eth 是 Ethernet 的缩略 以太网络

[docker for windows 容器内网通过独立IP直接访问的方法](https://www.cnblogs.com/brock0624/p/9788710.html) 讲了在windows下通过配置 route 命令配置路由的方式，使得 docker 容器 可以被局域网发现

[docker network](https://www.jianshu.com/p/04b33284f742) 对官网的文旦进行了一翻译

ip addr 输出的 linux IP 信息都是啥含义，参见文章：[透过ip addr看ip](https://blog.csdn.net/ZhanShen2015/article/details/82560627)

图解的一些概念，可以 [docker 网络配置](https://www.jianshu.com/p/d84cdfe2ea86)

[Docker的网络类型和固定IP设置][Macvlan]
[docker 官网 macvlan 配置说明][docker 官网 macvlan]

[Macvlan]:https://www.cnblogs.com/milton/p/9858955.html

[docker 官网 macvlan]:https://docs.docker.com/network/macvlan/

[Docker容器通过独立IP暴露给局域网的方法][Toolbox 通过 ip 暴露容器方案]
[Docker 容器使用宿主机同网段IP](https://www.jianshu.com/p/4d8605bea2bb)  - 实话实说，目前还看不懂
[Docker第二波][Docker第二波]
[Docker第三波][Docker第三波]

[Toolbox 通过 ip 暴露容器方案]:https://blog.csdn.net/lvshaorong/article/details/69950694
[Docker第二波]:https://note.youdao.com/ynoteshare1/index.html?id=76a0d0e23f629c48743e54d8c4ff83e9&type=note
[Docker第三波]:https://note.youdao.com/ynoteshare1/index.html?id=eba1f18eb65648e628ce2dcc4cef635c&type=note

## docker 数据持久

在 mysql 官网上看到，在 docker 中部署 mysql 的时候，采用的是 --mount 命令完成持久化；这个以前没有了解过，于是查阅资料：

[Docker数据持久之volume和bind mount](https://blog.csdn.net/docerce/article/details/79265858) 在这篇文章中，介绍了三种持久化的方案；表达了2个区别：

+ Bind mount会覆盖容器中的文件，而volume mount则不会，即如果容器中已有文件，则会将文件同步到主机的目录上。
+ mount 方式与Linux系统的mount方式很相似，即是会覆盖容器内已存在的目录或文件，但并不会改变容器内原有的文件，当umount后容器内原有的文件就会还原。

[docker volumes 中 -v 和 -mount 区别](http://einverne.github.io/post/2018/03/docker-v-and-mount.html) 在这篇文章中说，没啥区别

注意了，run 命令下 -v 和 --mount 是等效的；区别另外一种持久化方案 bind mounts 见官网 [Use bind mounts](https://docs.docker.com/storage/bind-mounts/)

### run -v 参数用法总结

#### 宿主主机路径（文件）与容器绑定

详见 官方文档 [docker run][docker run official]  -v 参数的使用相关章节

```sh
docker run -v [宿主主机文件系统路径]:[容器文件系统路径]
```

如果宿主主机中路径不存在，docker 会自动创建这个路径

[docker run official]:https://docs.docker.com/engine/reference/commandline/run/

#### 绑定卷

详见 官方文档 [use volumes][use volumes official]

```sh
docker run -v [卷名称]:[容器文件系统路径]
```

[use volumes official]:https://docs.docker.com/storage/volumes/

## docker 默认存储路径位置迁移

在实践中，通常需要把 docker 存储的路径放到一个比较大的磁盘中，避免占用系统盘过多的空间，采用这篇文章的方式 [Docker修改默认存储位置](https://blog.csdn.net/u010825468/article/details/78980886)

摘录如下：

```sh
# 确认当前 docker root dir 位置
docker info | grep 'Docker Root Dir'    // 默认输出结果为：Docker Root Dir: /var/lib/docker
# 移动到 docker root dir 路径上一层
cd /var/lib
#停止docker服务
root@xxxxxx:/var/lib# service docker stop 
#备份原目录
root@xxxxxx:/var/lib# cp -a docker{,_bak}
#拷贝数据到新位置
root@xxxxxx:/var/lib# cp -a docker/ {new_location}/
#建立软连接：
root@xxxxxx:/var/lib# rm -rf docker/
root@xxxxxx:/var/lib# ln -s {new_location}/ docker
#启动docker
root@xxxxxx:/var/lib# service docker start
#检查移动后数据是否完整
root@xxxxxx:/var/lib# docker images
root@xxxxxx:/var/lib# docker ps -a
#如果docker完整并可用，删除备份
root@xxxxxx:/var/lib# rm -rf docker_bak/
```

## docker 使用宿主机 IP 访问容器

增加防火墙配置

```sh
# 整体放开 docker 的防火墙限制
sudo firewall-cmd --permanent --zone=trusted --add-interface=docker0
# xxxx改为你希望的端口号
# sudo firewall-cmd --permanent --zone=trusted --add-port=xxxx/tcp
sudo firewall-cmd --reload
```

找了很多文章，终于看到一篇讲到这个主题，在 centos7 下面通过配置 firewall 实现 docker 容器通过宿主机 IP 和具体端口号访问宿主机上的其他容器，详见 [docker、firewalld和iptables之间的关系](https://www.jianshu.com/p/10c467600ef9)