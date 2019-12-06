# harbor 知识收集



# 安装

[CentOS7安装harbor](https://www.jianshu.com/p/4e929653eb95)

> 大多数时候，只需要修改hostname属性和https证书即可。配置完成之后再当前目录下执行./prepare,再执行./install.sh。
>
> Harbor没有证书的配置非常麻烦。

[harbor镜像仓库的安装与使用](https://blog.csdn.net/ZZY1078689276/article/details/102742509)

# 相关组件

## Clair

[容器静态安全漏洞扫描工具Clair介绍](https://zhuanlan.zhihu.com/p/43547242)

> 根据绿盟2018年3月的研究显示，目前Docker Hub上的镜像76%都存在漏洞，其研究人员拉取了Docker Hub上公开热门镜像中的前十页镜像，对其使用Docker镜像安全扫描工具Clair进行了CVE扫描统计。结果显示在一百多个镜像中，没有漏洞的只占到24%，包含高危漏洞的占到67%。很多我们经常使用的镜像都包含在其中，如：Httpd,Nginx,Mysql等等。