# alpine 知识收集

[TOC]

# 参考资料

[Alpine Linux](https://www.cnblogs.com/testzcy/p/7531281.html)

> Alpine Linux还提供了自己的包管理工具apk，可以在其网站上 [查询](https://pkgs.alpinelinux.org/packages)

[解决Alpine镜像安装软件速度慢的问题](https://www.wanghaiqing.com/article/d72e5407-f67d-4325-8fc9-08c9f2a97539/)

> ```dockerfile
> RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories
> RUN apk add --update-cache bash
> ```

