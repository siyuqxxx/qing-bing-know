# 支持 git 版本管理的 maven 项目

[TOC]

# 参考资料

《maven实战》
[Nexus Repository Manager OSS 3.x - Windows 安装配置](https://blog.csdn.net/clanguage2012/article/details/86600473)

[Nexus 3.X（Maven仓库私服）仓库迁移与备份](https://www.cnblogs.com/nethrd/p/9554163.html)

[使用Maven Release， 怎么跳过单元测试](https://hacpai.com/article/1457316942278)

> 最终的发布命令是
>
> mvn release:perform -Darguments="-Dmaven.test.skip=true -Dmaven.javadoc.skip=true"
>
>  关于命令参数的解释，可以参考 [mvn release:perform skip test](https://hacpai.com/forward?goto=http%3A%2F%2Fstackoverflow.com%2Fquestions%2F8685100%2Fhow-can-i-get-maven-release-plugin-to-skip-my-tests)


# pom.xml

```xml
<scm>
    <connection>scm:git:ssh://git@10.11.18.201:43022/oa-group/oa-privilege.git</connection>
    <tag>HEAD</tag>
</scm>
```

# 使用 maven 发布版本

```sh
mvn release:prepare -Darguments="-Dmaven.test.skip=true"
```



# 不需要交互设置版本号

[Apache Maven Release Plugin插件详解](https://blog.csdn.net/taiyangdao/article/details/82658799)

> 非交互模式
> 
> ```sh
> mvn -B release:prepare release:perform
> # 或
> mvn --batch-mode release:prepare release:perform
> ```
>
> 