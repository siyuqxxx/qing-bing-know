# harbor 知识收集



# 安装

[CentOS7安装harbor](https://www.jianshu.com/p/4e929653eb95)

> 大多数时候，只需要修改hostname属性和https证书即可。配置完成之后再当前目录下执行./prepare,再执行./install.sh。
>
> Harbor没有证书的配置非常麻烦。

[harbor镜像仓库的安装与使用](https://blog.csdn.net/ZZY1078689276/article/details/102742509)

[使用harbor和nexus作为docker registry](https://blog.csdn.net/weixin_34088838/article/details/91378874)

> harbor 的优势很明显，特别是可以自建文件夹进行分组这点就非常好。其实说实话，作为一个私有的镜像仓库，harbor 已经做得很好的了，唯一的缺点是它无法帮你下载镜像。
>
> 也可能并不是缺点，只是定位不同，而在这一点上 nexus 就很好的做到了，但是 nexus 除了这点外，其他好像没什么值得称道的地方了。
>
> 为什么需要 Registry 帮你下载镜像呢？因为在 kubernetes 环境下，你肯定有去公网拉镜像的需求，无论是官方还是非官方。你不可能因为这个而特地给你的所有 node 节点开通外网访问吧，这样风险太多且不可控。在我看来，整个 kubernetes 集群都不能访问外网。
>
> 这时 nexus 就有了用武之地了，当你需要拉公网镜像的时候，你只要向它发起请求，它如果本地没有，就会自动去你配置的镜像仓库下载，下载完成之后先在本地缓存一份，然后发送给你。

# 相关组件

## Clair

[容器静态安全漏洞扫描工具Clair介绍](https://zhuanlan.zhihu.com/p/43547242)

> 根据绿盟2018年3月的研究显示，目前Docker Hub上的镜像76%都存在漏洞，其研究人员拉取了Docker Hub上公开热门镜像中的前十页镜像，对其使用Docker镜像安全扫描工具Clair进行了CVE扫描统计。结果显示在一百多个镜像中，没有漏洞的只占到24%，包含高危漏洞的占到67%。很多我们经常使用的镜像都包含在其中，如：Httpd,Nginx,Mysql等等。