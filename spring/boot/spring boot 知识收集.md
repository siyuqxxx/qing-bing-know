# spring boot 知识收集

[TOC]

# 参考资料

[SpringBoot 整合mybatis (手写mapper.xml)](https://www.jianshu.com/p/d513339980c7)

[SpringBoot集成Mybatis中找不到Mapper层的接口](https://bbs.csdn.net/topics/392420803?page=1)


# 事务管理

[透彻的掌握 Spring 中@transactional 的使用](https://www.ibm.com/developerworks/cn/java/j-master-spring-transactional-use/)

[Spring Boot的事务管理注解@EnableTransactionManagement的使用](https://blog.csdn.net/u010963948/article/details/79208328)

# 日志

[SpringBoot Logging配置](https://www.liangzl.com/get-article-detail-6974.html)

[springboot在yml内的日志配置](https://www.oschina.net/question/3536851_2281935)

> [application.properties <--> application.yml 互转工具](http://www.toyaml.com/  )

# 校验

[Spring Boot 进行Bean Validate和Method Validate](https://blog.csdn.net/adsadadaddadasda/article/details/80831832)

[使用JSR 303和AOP简化你的接口开发](https://blog.csdn.net/chaijunkun/article/details/44854071)

[一起来学SpringBoot | 第十九篇：轻松搞定数据验证（一）](http://www.spring4all.com/article/1224)

[一起来学SpringBoot | 第二十篇：轻松搞定数据验证（二）](http://www.spring4all.com/article/1225)

[一起来学SpringBoot | 第二十一篇：轻松搞定数据验证（三）](http://www.spring4all.com/article/1228)

# junit

[MockMvc control层单元测试 参数传递问题](https://blog.csdn.net/wang_muhuo/article/details/84655577)

[HTTP PUT vs HTTP PATCH in a REST API](https://www.baeldung.com/http-put-patch-difference-spring)

[SpringMVC 测试 mockMVC](https://www.cnblogs.com/lyy-2016/p/6122144.html)

[MockMVC - 基于RESTful风格的SpringMVC的测试](https://www.jianshu.com/p/91045b0415f0)

> 好文章

[Java Code Examples for org.springframework.test.web.servlet.result.MockMvcResultMatchers](https://www.programcreek.com/java-api-examples/index.php?api=org.springframework.test.web.servlet.result.MockMvcResultMatchers)

> 好文章，太不错了，超级多的例子

[github/json-path/JsonPath](https://github.com/json-path/JsonPath)

# 问题解决

[jdk9+ spring5+ 报反射错误的屏蔽方法](https://my.oschina.net/u/698683/blog/1922830)
> 根本原因是使用 jdk9+ 时，模块系统会检查 不合法的入口
> This happens because the new JDK 9 module system detected an illegal access that will be disallowed sometime in the (near) future. 
>
> 添加虚拟机参数 `--add-opens java.base/java.lang=ALL-UNNAMED`

[如何对 JPA 或者 MyBatis 进行技术选型](https://blog.csdn.net/yangsnow_rain_wind/article/details/79650616)

[MyBatis与Spring Data关于@Repository注解的注意事项](https://blog.csdn.net/mr_ystreet/article/details/82790741)

> 似乎并没有更进一步说明

需要确认一下 spring-boot-devtools 

[使用spring-boot-devtools实现热部署及热更新](https://jingyan.baidu.com/article/90808022025b13fd90c80f5c.html)

需要确认一下 @Entity @Data @Repository