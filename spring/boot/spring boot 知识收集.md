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

# 认证

[Spring Cloud入门教程(十一)：微服务安全(Security)](https://www.jianshu.com/p/664c71b6681a)

> 在2016年David Borsos在伦敦的微服务大会上提出了以下四种方案：
>
> 1. **单点登录(SSO)**: 每个微服务都需要和认证服务交互，但这将产生大量非常琐碎的网络流量和重复的工作，当动在应用中存在数十个或更多微服务时，该方案的弊端就非常明显;
> 2. **分布式会话(Session)方案**: 该方案将用户认证的信息存储在共享存储中(如：Redis)，并使用用户会话的ID作为key来实现的简单分布式哈希映射。当用户访问微服务时，可以通过会话的ID从从共享存储中获取用户认证信息。该方案在大部分时候非常不错，但其主要缺点在于共享存储需要一定保护机制，此时相应的实现就会相对复杂;
> 3. **客户端令牌(Token)方案**: 令牌在客户端生成，并由认证服务器进行签名，令牌中包含足够的信息，以便各微服务可以使用。令牌会附加到每个请求上，为微服务提供用户身份验证。该解决方案的安全性相对较好，但由于令牌由客户端生成并保存，因此身份验证注销非常麻烦，一个折衷解决方案就是通过短期令牌和频繁检查认证服务来验证令牌是否有效等。对于客户端令牌JSON Web Tokens(JWT)是一个非常好的选择;
> 4. **客户端令牌与API网关结合**: 使用该方案意味着所有请求都通过网关，从而有效地隐藏了微服务。在请求时，网关将原始用户令牌转换为内部会话。这样也就可以网关对令牌进行注销，从而解决上一种方案存在的问题。

[基于 Spring Session & Spring Security 微服务权限控制](https://segmentfault.com/a/1190000019593308)

> 这个比较符合现在项目的认证机制

## OAuth2.0

关键词：spring cloud security oauth2.0



[SpringCloud微服务间安全调用实现（SpringSecurity+Oauth2+Jwt）](https://blog.csdn.net/q438944209/article/details/82920461)

[Spring Cloud入门教程(十一)：微服务安全(Security)](https://www.jianshu.com/p/664c71b6681a)

> 觉得写得比较好，不过感觉有些内容并没有写得很细致

[基于Spring Security实现权限管理系统](https://blog.csdn.net/cloume/article/details/83790111)

> 仅供参考，文章写得比较清晰

[Spring cloud微服务实战（一）基于OAUTH2.0统一认证授权的微服务基础架构](https://blog.csdn.net/w1054993544/article/details/78932614)

> 这个是一个架构方面的全景图，后面进一步研究

[Spring Cloud Security OAuth2（一）：搭建授权服务](https://segmentfault.com/a/1190000014687027)

> 觉得比较系统，但是还没有仔细看

[Spring Cloud Security 之 OAuth2.0](https://www.cnblogs.com/moues/p/10827834.html)

> 这里有一个流程，感觉有用，但是比较抽象，应该后面业务比较熟悉以后能够掌握

[Spring Cloud【Spring Security OAuth2】授权认证](https://segmentfault.com/a/1190000014129319)

> 讲到了一个工具 Spring Boot Admin 后面研究

[深入理解Spring Cloud Security OAuth2及JWT](https://www.jianshu.com/p/cb886f995e86)

> 讲了一个概念 使用 OAuth2.0 做 sso

[Spring cloud OAuth2 and JWT](https://www.jianshu.com/p/4089c9cc2dfd)

> 认证的流程图画得比较好

[官方指导](https://projects.spring.io/spring-security-oauth/docs/Home.html)

> [spring-projects/spring-security-oauth](https://github.com/spring-projects/spring-security-oauth)

[Spring Security OAuth2集成短信验证码登录以及第三方登录](https://segmentfault.com/a/1190000014371789)

> 基于SpringCloud做微服务架构分布式系统时，OAuth2.0作为认证的业内标准，Spring Security OAuth2也提供了全套的解决方案来支持在Spring Cloud/Spring Boot环境下使用OAuth2.0，提供了开箱即用的组件。但是在开发过程中我们会发现由于Spring Security OAuth2的组件特别全面，这样就导致了扩展很不方便或者说是不太容易直指定扩展的方案，例如：
>
> 1. 图片验证码登录
> 2. 短信验证码登录
> 3. 微信小程序登录
> 4. 第三方系统登录
> 5. CAS单点登录

[Spring Cloud下基于OAUTH2认证授权的实现](https://www.iteye.com/blog/wiselyman-2379419)

> 先收藏了

[Spring Security Oauth2.0 实现短信验证码登录示例](https://www.jb51.net/article/132736.htm)

> 先收藏了

[oauth2.0 实现spring cloud nosession](https://my.oschina.net/giegie/blog/1524723)

> 觉得可以参考这个做一个 demo

[从零开始的Spring Security Oauth2（一）](http://blog.didispace.com/spring-security-oauth2-xjf-1/)

[从零开始的Spring Security Oauth2（二）](http://blog.didispace.com/spring-security-oauth2-xjf-2/)

[从零开始的Spring Security Oauth2（三）](http://blog.didispace.com/spring-security-oauth2-xjf-3/)

> 这里面有一个关键的内容，各接口需要使用认证中携带的用户信息，
>
> 嗯，不错，还有几篇进阶文章

[SpringCloud(七)：搭建OAuth2认证授权服务](https://www.jianshu.com/p/c274f4acadb5)

[SpringCloud+SpringBoot+OAuth2+Spring Security+Redis实现的微服务统一认证授权](https://blog.csdn.net/WYA1993/article/details/85050120)

## 使用拦截器校验请求中 token 进行认证

关键词： spring boot 拦截器 



[Spring Boot 中使用拦截器](https://blog.csdn.net/taojin12/article/details/88342576)

> 使用注解，实现一些方法不需要拦截
>
> 在 Spring Boot 2.0 之前，我们都是直接继承 WebMvcConfigurerAdapter 类，然后重写 addInterceptors 方法来实现拦截器的配置。但是在 Spring Boot 2.0 之后，该方法已经被废弃了（当然，也可以继续用），取而代之的是 WebMvcConfigurationSupport 方法

[Spring Boot 优雅的配置拦截器方式](https://my.oschina.net/bianxin/blog/2876640)

> 采用 继承父类 HandlerInterceptorAdapter
>
> 使用注解，实现需要登录验证的方法

[Spring Boot2.0 设置拦截器](https://www.cnblogs.com/rolandlee/p/9580492.html)

> 采用 实现接口 HandlerInterceptor 

# scope

[spring 组件@Scope(request,session)示例](https://www.bbsmax.com/A/Vx5MvQ97dN/)

> 那在谈到具体的示例前，我先分享下对这两种场景的使用心得，以便与各位看官进行思想上的神交! 我们都知道B/S站点运行起来后，是一个多线程的运行环境。**每个客户端登录都会产生一个session会话,它的生命周期 从登录系统到 session过期，期间session上存储的信息都是有效可用的，我习惯于叫它会话级的缓存，像用户登录的身份信息我们一般都会绑定到这个session上**。这里我们要讲的@Scope("session")，就是spring提供出来的一个会话级bean方案,在这种模式下，用spring的DI功能来获取组件,可以做到在会话的生命周期中这个组件只有一个实例。接下来再说请求(request),http协议的处理模型,从客户端发起request请求，到服务端的处理，最后response给客户端，我们称为一次完整的请求。在这样的一次请求过程中，我们的服务站可能要串行调用funcA->funcB->funcC·... 这样的一串函数，假如我们的系统需要做细致的权限校验(用户权限,数据权限)，更可怕的是funcA,funcB,funcC是3个人分别实现的，而且都要做权限校验。那么极有可能会出现3个人各连接了一次数据库，读取了同一批权限数据。这里想象一下，假如一个数据读取要花2秒,那么3个方法就要花费6秒的处理时间。**但实际上这些数据只用在这个请求过程中读取一次,缓存在request上下文环境中，我习惯称之为线程级缓存。关于线程级缓存java有ThreadLocal方案，像Hibernate的事务就是用这种方案维持一次执行过程中数据库连接的唯一**。当然，今天要讲的@Scope("request")也可以做到这种线程级别的缓存。下面我们看看具体的测试示例