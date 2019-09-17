# natapp 常用操作

[TOC]

# windows

## 配置为后台服务

[Natapp(Ngrok) Windows下注册为服务,开机启动&amp;后台运行](https://natapp.cn/article/windows_service) - natapp 官方说明文档3

# Linux

## 配置为后台服务

[linux后台运行natapp(ngrok)教程](https://natapp.cn/article/nohup) - natapp 官方说明文档3

## 配置为开机启动

在 /opt/installer/natapp 目录下放置一个 natapp linux 可执行文件

设置执行权限 

```shell
chmod +x /opt/installer/natapp/natapp
```

创建 systemctl 脚本 

```shell
vi /usr/lib/systemd/system/natapp.service
```

natapp.service 内容如下，注意 natapp 可执行文件的 `路径` 和 `authtoken`

```shell
# Centos 7
# 存放位置 /usr/lib/systemd/system
# 开启 systemctl start natapp
# 关闭 systemctl stop natapp
# 开机启动 systemctl enable natapp
# 取消开机启动 system disable natapp

[Unit]
Description=NatApp Service
Wants=network-online.target
After=network.target


Type=simple
ExecStart=/opt/installer/natapp/natapp -authtoken=e30882b1eb303d71 -log=stdout
# Suppress stderr to eliminate duplicated messages in syslog. NM calls openlog()
# with LOG_PERROR when run in foreground. But systemd redirects stderr to
# syslog by default, which results in logging each message twice.
StandardOutput=syslog
StandardError=null


[Install]
WantedBy=multi-user.target
```

设置开机启动，并启动natapp

```sh
# 设置开机启动
systemctl enable natapp

# 启动 natapp 服务
systemctl start natapp
```