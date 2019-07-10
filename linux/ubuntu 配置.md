# ubuntu 配置

[TOC]

## 基本配置

### 虚拟机硬件配置

| 序号 | 硬件 | 指标     |
| ---- | ---- | -------- |
| 1    | 内存 | 6G       |
| 2    | cup  | 2个      |
| 3    | 显存 | 32M      |
| 4    | 硬盘 | 64G      |
| 5    | 网卡 | 桥接网卡 |

#### 共享剪切板

[VirtualBox运行Ubuntu 18.04中开启共享文件夹，共享粘贴板，拖放](https://ywnz.com/linuxjc/2411.html)

### 常规软件

| 序号 | 软件      | 备注            |
| ---- | --------- | --------------- |
| 1    | 增强工具  | virtualbox 自带 |
| 2    | chrome    |                 |
| 3    | jdk11     | oracle 版本的   |
| 4    | maven     |                 |
| 5    | eclipse   |                 |
| 6    | git       |                 |
| 7    | typora    | markdown editor |
| 8    | gitkraken | git GUI client  |
| 9    | lantern   | 梯子            |

#### 更新 apt 源

+ 软件 》 ubuntu 软件 》 下载自 》 其他站点... 》 选择最佳服务器

+ 软件更新器

#### 卸载 firefox

[ubuntu系统怎样卸载火狐浏览器](https://blog.csdn.net/qq_41149269/article/details/81175948)，摘要如下：

```sh
# 检查包含的 firefox 程序
dpkg --get-selections | grep firefox

# 删除 firefox
sudo apt-get purge firefox firefox-locale-en firefox-locale-zh-hans unity-scope-firefoxbookmarks
```

#### 安装 oracle-jdk11

[如何在Ubuntu 18.04/18.10中安装Oracle Java 11](https://www.linuxidc.com/Linux/2018-11/155562.htm)

```sh
sudo add-apt-repository ppa:linuxuprising/java
```

执行上面的命令，可以在命令行中可以看到这个 url: [new-oracle-java-11-installer-for-ubuntu](https://www.linuxuprising.com/2019/06/new-oracle-java-11-installer-for-ubuntu.html)，核心步骤摘录如下：

```sh
# 背景
# 正如许多人所知，Oracle Java 需要登录 Oracle 帐户才能下载大多数版本（除 Oracle Java 12 外）。
# 由于无法再从 Oracle 直接下载 Oracle Java 11，直接通过 apt 安装程序不再有效，因此，采用了一个新的安装程序，要求用户创建 Oracle 帐户，下载 Oracle Java 11 .tar.gz （同样 版本作为安装程序），并将存档放在 /var/cache/oracle-jdk11-installer-local/ 中。 在此之后，您可以安装 oracle-java11-installer-local 软件包，它将为您设置Oracle Java 11。

# 创建临时目录
sudo mkdir -p /var/cache/oracle-jdk11-installer-local/

# 拷贝从 oracle 官网下载的 jdk 到临时目录中
sudo cp jdk-11.0.3_linux-x64_bin.tar.gz /var/cache/oracle-jdk11-installer-local/

# 删除旧的 jdk；如果没有，跳过
sudo apt purge oracle-java11-installer

# 添加 apt 源
sudo add-apt-repository ppa:linuxuprising/java

# 更新源
sudo apt-get update

# 安装 jdk
sudo apt install oracle-java11-installer-local

# 设置当前 jdk 为默认
sudo apt install oracle-java11-set-default-local

# 测试安装结果
java -version
```

#### 安装 maven

```sh
sudo apt install maven
```