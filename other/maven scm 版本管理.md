# maven scm 版本管理

[TOC]

# pom.xml

```xml
<scm>
    <connection>scm:git:ssh://git@10.11.18.201:43022/oa-group/oa-privilege.git</connection>
    <tag>HEAD</tag>
</scm>
```

# 使用 maven 发布版本

```sh
mvn -B release:prepare -Darguments="-Dmaven.test.skip=true"
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