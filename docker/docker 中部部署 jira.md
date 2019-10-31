# docker 中部部署 jira

[TOC]

# 参考资料

[通过Docker安装JIRA和Confluence（破解版）](https://www.jianshu.com/p/b95ceabd3e9d)

> Docker镜像 [Github链接](https://github.com/cptactionhank)

# 镜像

```sh
docker pull cptactionhank/atlassian-jira:latest
docker run --name jira -d -p 40007:8080 cptactionhank/atlassian-jira:latest
```

# 版本区别

[JIRA Core、JIRA Software、JIRA Service Desk的区别](https://blog.csdn.net/cabinhe/article/details/78165832)