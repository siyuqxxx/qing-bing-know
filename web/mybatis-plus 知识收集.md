# mybatis-plus 知识收集

[TOC]

# 参考资料

[mybatis-plus id主键生成的坑](https://blog.csdn.net/qq_34208844/article/details/88819467)

> 解决问题：
>
> 由于`mybatis-plus`会自动插入一个`id`到实体对象, 不管你封装与否, 所以有时候导致一些意外的情况发生：默认是生成一个长数字字符串(编码不同可能结尾带有字母)
>
> ```
> @TableId(value = "id",type = IdType.AUTO)
> ```



## 分页

官方： [分页插件](https://mp.baomidou.com/guide/page.html)

```java
//Spring boot方式
@Configuration
@MapperScan("com.baomidou.cloud.service.*.mapper*")
public class MybatisPlusConfig {

    /**
     * 分页插件
     */
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}
```



[[Mybatis-Plus] Springboot2集成Mybatis-Plus分页插件](https://www.jianshu.com/p/84f05ac3aeb4)

> 讲解了分页插件的配置
>
> application.yml 的配置可以参考
>
> 打印 sql 配置

[spring boot整合mybatis+mybatis-plus](https://www.cnblogs.com/lianggp/p/7573653.html)

> 包含一个比较不错的 yml 配置
>
> 包含 mybatis-plus generator 配置

## 代码生成器

官方： 

[代码生成器](https://mp.baomidou.com/guide/generator.html)

[代码生成器配置](https://mp.baomidou.com/config/generator-config.htm)