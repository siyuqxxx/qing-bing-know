# spring boot + mybatis-plus 知识收集

[TOC]

# 参考资料

[mybatis-plus的使用 ------ 进阶](https://www.jianshu.com/p/a4d5d310daf8)

# pom.xml 配置

[快速开始](https://mp.baomidou.com/guide/quick-start.html) - 官网

```xml
<!--mybatis-plus 核心依赖-->
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus-boot-starter</artifactId>
  <version>3.2.0</version>
</dependency>

<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.2.0</version>
</dependency>
```

# 代码生成器

官方： 

[代码生成器](https://mp.baomidou.com/guide/generator.html)

[代码生成器配置](https://mp.baomidou.com/config/generator-config.htm)

# 生成主键

[mybatis-plus id主键生成的坑](https://blog.csdn.net/qq_34208844/article/details/88819467)

> 解决问题：
>
> 由于`mybatis-plus`会自动插入一个`id`到实体对象, 不管你封装与否, 所以有时候导致一些意外的情况发生：默认是生成一个长数字字符串(编码不同可能结尾带有字母)
>
> ```
> @TableId(value = "id",type = IdType.AUTO)
> ```

# 分页

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

# 自动填充 @TableField

[Mybatis-Plus公共字段自动填充注解使用说明@TableField、@Version](https://blog.csdn.net/tianmaxingkonger/article/details/84851206)

[MyBatisPlus怎么忽略映射字段](https://www.cnblogs.com/yifanSJ/p/9098166.html)

# 修改主键

[MyBatis-Plus-Example](https://github.com/fengwenyi/MyBatis-Plus-Example)

> 这个例子可以下载下来看一下

[Mybatis-Plus的填坑之路 - Lynwood/wunian7yulian](https://www.cnblogs.com/wunian7yulian/p/10276642.html)

> ```java
> public void abstractWrapperTest_Set() {
>     UpdateWrapper<User> userUpdateWrapper = new UpdateWrapper<>();
>     // 更新id==11 的age 为19
>     userUpdateWrapper.set("age",19).eq("id",11);
>     // 附加更新内容 name 设置
>     User user = new User();
>     user.setName("Lynwood1");
>     userMapper.update(user,userUpdateWrapper);
>     /**
>      *          ==>  Preparing: UPDATE user SET name=?, age=? WHERE id = ?
>      *          ==> Parameters: Lynwood1(String), 19(Integer), 11(Integer)
>      *          <==    Updates: 1
>      */
> 
>     UpdateWrapper<User> userUpdateWrapper1 = new UpdateWrapper<>();
>     userUpdateWrapper1.setSql("name='wunian7yulian'").eq("id",11);
>     // 附加更新内容为空时  不能传入null  需要传入空实体对象
>     userMapper.update(new User(),userUpdateWrapper1);
>     /**
>      *      ==>  Preparing: UPDATE user SET name='wunian7yulian' WHERE id = ?
>      *      ==> Parameters: 11(Integer)
>      *      <==    Updates: 1
>      */
> }
> ```
>
> 



```java
public void id2UUID() {
    LambdaUpdateWrapper<Org> wrapper = Wrappers.<Org>lambdaUpdate()
        .set(Org::getId, UUID.randomUUID().toString().replace("-", ""))
        .eq(Org::getId, "1");
    super.update(new Org(), wrapper);
}
```

> 

# OR

[MybatisPlus QueryWrapper and or 连用](https://blog.csdn.net/u011229848/article/details/81902398)