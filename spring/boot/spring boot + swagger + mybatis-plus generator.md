# spring boot + swagger + mybatis-plus generator

[TOC]

# 参考资料

[github/SpringForAll/spring-boot-starter-swagger](https://github.com/SpringForAll/spring-boot-starter-swagger)

> 这是一个整合套件，readme.xml 中包含了详细配置说明，包括：
>
> + pom.xml 配置
> + 启动类注解配置
> + application.yml 配置

# pom.xml

```xml
<!-- swagger api 生成器套件 -->
<dependency>
  <groupId>com.spring4all</groupId>
  <artifactId>swagger-spring-boot-starter</artifactId>
  <version>1.9.0.RELEASE</version>
</dependency>
```



# 启动类开启 swagger

在启动类上添加 `@EnableSwagger2Doc` 注解，参考

```java
import com.spring4all.swagger.EnableSwagger2Doc;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@EnableSwagger2Doc
@SpringBootApplication
public class BasicApplication {

    public static void main(String[] args) {
        SpringApplication.run(BasicApplication.class, args);
    }
}
```

# 定义静态资源映射目录

[SpringBoot集成Swagger2配置及出现的问题记录](https://blog.csdn.net/sinat_39456789/article/details/86673628)

> 原因：
>
> 因为swagger-ui.html相关的所有前端静态文件都在springfox-swagger-ui-x.x.x.jar里面，在yml文件中配置了mvc.static-path-pattern: /static/** 导致swagger-ui.html相关的所有前端静态文件映射不到。
> 解决方案：
>
> 定义静态资源映射目录，addResoureHandler指的是对外暴露的访问路径，addResourceLocations指的是文件放置的目录
>
> 

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("swagger-ui.html")
                .addResourceLocations("classpath:/META-INF/resources/");
    }
}
```

# application.yml 配置

这个配置随环境走，由具体的环境控制是否开关

```yml
swagger:
  enabled: true
```

这个配置是公共的，不展示 spring boot 自带`/error`请求路径下的 api 接口。

```yml
swagger:
  exclude-path: /error
  authorization:
    key-name: Authorization
```

# mybatis-plus generator 配置

```java
/**
 * 全局配置
 *
 * @param projectPath
 * @return
 */
private static GlobalConfig createGlobalCfg(String projectPath) {
    GlobalConfig gc = new GlobalConfig();
    gc.setOutputDir(projectPath + "/src/main/java");
    gc.setAuthor("rbzy");
    gc.setOpen(false);
    // 实体属性 Swagger2 注解
    gc.setSwagger2(true);
    return gc;
}
```

# 自定义拦截器配置

[spring boot 加入拦截器后swagger不能访问问题](https://blog.csdn.net/liu0bing/article/details/80826590)

> 不要拦截这些 `"/swagger-resources/**", "/webjars/**", "/v2/**", "/swagger-ui.html/**"`

[spring boot 集成swagger并且使用拦截器的配置问题](https://segmentfault.com/a/1190000018913038)

> 写的比较详细一些
>
> 另外提到了 /error，不拦截 spring boot 自带`/error`请求路径下的 api 接口。

```java
import com.wdzggroup.rbzy.oa.interceptor.LoginInterceptor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.Ordered;
import org.springframework.web.servlet.config.annotation.*;

/**
 * 加载静态资源类
 * liuzhize 2019年3月7日下午3:25:49
 */
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(LoginInterceptor())
                .addPathPatterns("/**")
                .excludePathPatterns("/error")
                .excludePathPatterns("/swagger-resources/**", "/webjars/**", "/v2/**", "/swagger-ui.html");
    }

    @Bean
    public LoginInterceptor LoginInterceptor() {
        return new LoginInterceptor();
    }
}
```

# swagger 生成接口的鉴权问题

详见参考资料中，[github/SpringForAll/spring-boot-starter-swagger](https://github.com/SpringForAll/spring-boot-starter-swagger) swagger 套件的 readme.md 通过配置实现鉴权，摘录如下

```yml
swagger:
  authorization:
    key-name: Authorization
```

