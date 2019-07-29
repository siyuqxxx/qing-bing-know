# CentOS7 安装 git 最新版本

[TOC]

# 参考资料

[在centos6.5 64bit环境下安装最新版本的git](https://blog.csdn.net/ronnyjiang/article/details/51222573) - csdn

> 我们为什么不用命令直接去安装？还要单独下载git的安装包来编译安装呢？
>
> 这是因为linux系统库中git的版本都比较老，还停留在V1.*.*的版本,为了更好的支持git的性功能特性，我们应该去安装git官网比较新的版本，现在官网git已经是2.8.0版本了。我们所示想要获取最新的git版本，那就只能下rpm包或者用源码来实现。

[CentOS7 执行yum 命令出错](https://blog.csdn.net/zhousenshan/article/details/53140979) - csdn

[Linux同步网络时间](https://blog.csdn.net/weiyuefei/article/details/71514133) - csdn

# 使用三方仓库安装

[Download for Linux and Unix](https://git-scm.com/download/linux) - git 官网

> **Red Hat Enterprise Linux, Oracle Linux, CentOS, Scientific Linux, et al.**
>
> RHEL and derivatives typically ship older versions of git. You can [download a tarball](https://www.kernel.org/pub/software/scm/git/) and build from source, or use a 3rd-party repository such as [the IUS Community Project](https://ius.io/) to obtain a more recent version of git.
>
> RHEL和衍生产品通常都会发布旧版本的git。 您可以从源代码下载tarball并构建，或使用第三方存储库（如IUS社区项目）来获取更新版本的git。

[什么是EPEL 及 Centos上安装EPEL](https://www.cnblogs.com/gaoyuechen/p/7683471.html)

> 什么是EPEL 及 Centos上安装EPEL
>
> RHEL以及他的衍生发行版如CentOS、Scientific Linux为了稳定，官方的rpm repository提供的rpm包往往是很滞后的，当然了，这样做这是无可厚非的，毕竟这是服务器版本，安全稳定是重点，官方的rpm repository提供的rpm包也不够丰富，很多时候需要自己编译那太辛苦了，而EPEL恰恰可以解决这两方面的问题。

[CentOS 7 使用IUS第三方源安装git2](https://blog.csdn.net/MaxWoods/article/details/89401889)

> ```sh
> curl https://setup.ius.io | sh
> yum install -y git2u
> git --version
> ```

# 源码安装

### 安装依赖软件

```sh
yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel asciidoc gcc perl-ExtUtils-MakeMaker
```

### 卸载 git （如果版本太老）

注意，安装完这些依赖以后，再做一次  `git --version` 检查会发现git是已经安装好的，但是版本很低，这个时候执行一次卸载。

```sh
[root@uatjenkins01 ~]# git --version
git version 1.7.1
[root@uatjenkins01 ~]# yum remove -y git
```

### 安装 git

```sh
[root@uatjenkins01 ~]# cd /usr/local/src/
[root@uatjenkins01 src]# wget https://www.kernel.org/pub/software/scm/git/git-2.18.0.tar.xz
[root@uatjenkins01 src]# tar -vxf git-2.18.0.tar.xz
[root@uatjenkins01 src]# cd git-2.18.0
[root@uatjenkins01 git-2.15.1]# make prefix=/usr/local/git all
[root@uatjenkins01 git-2.15.1]# make prefix=/usr/local/git install
[root@uatjenkins01 git-2.15.1]# ln -s /usr/local/git/bin/git /bin/git
[root@uatjenkins01 ~]# git --version
```

# 常见问题

## CentOS7 执行 yum 命令出错

或者，yum下载太慢也可以用下面方式处理

```sh
vi /etc/resolv.conf
nameserver 8.8.8.8
nameserver 114.114.114.114 // 新增

systemctl restart network.service // 重启网络服务
```

## wget: 未找到命令

在装数据库的时候发现无法使用wget命令，提示未找到命令

```sh
yum install -y wget
```

参见：[linux中wget未找到命令](https://blog.csdn.net/djj_alice/article/details/80407769) - csdn

## make[1]: 警告：检测到时钟错误。您的创建可能是不完整的。

```sh
yum install -y ntpdate
ntpdate -u ntp.api.bz
```

## gogs "git": executable file not found in $PATH

```sh
ln -s /usr/local/git/bin/git /bin/git
ln -s /usr/local/git/bin/git-cvsserver /bin/git-cvsserver
ln -s /usr/local/git/bin/gitk /bin/gitk
ln -s /usr/local/git/bin/git-receive-pack /bin/git-receive-pack
ln -s /usr/local/git/bin/git-shell /bin/git-shell
ln -s /usr/local/git/bin/git-upload-archive /bin/git-upload-archive
```