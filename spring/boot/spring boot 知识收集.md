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