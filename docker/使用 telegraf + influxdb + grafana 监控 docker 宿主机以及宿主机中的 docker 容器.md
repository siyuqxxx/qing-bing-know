# 使用 telegraf + influxdb + grafana 监控 docker 宿主机以及宿主机中的 docker 容器

[TOC]

# 参考资料

[docker 官网 telegraf 容器介绍](https://hub.docker.com/_/telegraf) - hub.docker.com
[dockerfile telegraf:1.9.5-alpine](https://github.com/influxdata/influxdata-docker/blob/adc5c37b7f11f730d9ef9c732cb10ce9b8bfc0d3/telegraf/1.9/alpine/Dockerfile) - github.com

[docker 官网 influxdb 容器介绍](https://hub.docker.com/_/influxdb) - hub.docker.com
[dockerfile influxdb:1.5.4-alpine](https://github.com/influxdata/influxdata-docker/blob/7f0f853c5a51c825b45a97bdb7189d366d64373c/influxdb/1.5/alpine/Dockerfile) - github.com

[docker 官网 grafana/grafana 容器介绍](https://hub.docker.com/r/grafana/grafana) - hub.docker.com
[grafana 官方 docker 安装指南](https://grafana.com/docs/installation/docker/) - grafana

[grafana+influxdb 监控](https://www.iteye.com/blog/san-yun-2394369)

# 配置

```sh
# 获取镜像
docker pull telegraf:1.9.5-alpine
docker pull grafana/grafana:6.2.5
docker pull influxdb:1.5.4-alpine

# 创建内部网络
docker network create influxdb

# 创建 influxdb
docker run -d --name=influxdb \
      -v influxdb-data:/var/lib/influxdb \
      -e ADMIN_USER="telegraf" \
      -e INFLUXDB_INIT_PWD="telegraf" \
      -e PRE_CREATE_DB="telegraf" \
      --net=influxdb \
      influxdb:1.5.4-alpine

# 创建 telegraf

## 设置 telegraf 配置文件
echo '
[[inputs.docker]]
  endpoint = "unix:///var/run/docker.sock"
  timeout = "10s"
  
[[outputs.influxdb]]
  urls = ["http://influxdb:8086"]
  database = "telegraf"
  username = "telegraf"
  password = "telegraf"
  retention_policy = ""
  write_consistency = "any"
  timeout = "5s"
' > telegraf.conf

## 创建 telegraf 容器
docker run -d --name=telegraf \
      --net=influxdb \
      -v /var/run/docker.sock:/var/run/docker.sock \
      -v $PWD/telegraf.conf:/etc/telegraf/telegraf.conf:ro \
      telegraf:1.9.5-alpine

# 创建 grafana
docker run -d --name=grafana \
  -p 40006:3000 \
  -v grafana-data:/var/lib/grafana \
  --net=influxdb \
  grafana/grafana:6.2.5
```

# influxdb 配置与注意事项

1. 启动命令：

```sh
docker run --name influxdb -d \
        -p 8086:8086 \
        -v /influxdb:/var/lib/influxdb \
        -e ADMIN_USER="telegraf" \
        -e INFLUXDB_INIT_PWD="telegraf" \
        -e PRE_CREATE_DB="telegraf" \
        --restart=always \
        docker.io/influxdb:latest
```

2. 修改日志数据保存策略:
    ①：docker exec -ti telegraf /bin/bash 
    ②：influx
    ③：查看show retention policies on telegraf
    ④：修改ALTER RETENTION POLICY "autogen" ON "telegraf" DURATION 4w DEFAULT
     （autogen代表策略名称，4w代表4周）