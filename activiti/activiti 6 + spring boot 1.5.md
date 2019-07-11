# activiti 6 + spring boot 1.5

[TOC]

## 参考资料

[Activiti User Guide V6.0.0](https://www.activiti.org/userguide/#springSpringBoot)

## 配置 spring boot 基础框架

### 不得不说的背景资料

[SpringBoot开发案例之整合Activiti工作流引擎](http://m.imooc.com/article/278542)

> 截止2019年2月1日 spring-boot 2.0 没有相关 activiti 的 starter-basic

[Activiti6完美集成SpringBoot2](https://www.jianshu.com/p/9b1dbb2c85e7)

> Activiti6 出来的时候，Springboot2 还没有出来，所以不能直接引用或者说,从源码上就不支持

### pom.xml (base)

作为基础框架 pom.xml 非常简单，仅仅包括以下内容

+ spring boot 版本：1.5.21.RELEASE
+ 包含一个 spring boot web 启动框架：spring-boot-starter-web
+ 包含一个 spring boot 测试框架：spring-boot-starter-test

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.21.RELEASE</version>
    <relativePath /> <!-- lookup parent from repository -->
  </parent>
  <groupId>com.wdzggroup.rbzy</groupId>
  <artifactId>oa-base</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <name>oa-base</name>
  <description>oa-base project for Spring Boot</description>

  <properties>
    <java.version>1.8</java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>

</project>
```



## 运行在 h2 中的 avtiviti

### 基本思想

+ 先用 H2 搭建一个开发环境出来；
+ 使用 H2 的原因：实际上，在开发阶段，数据无需真正的持久化

### 配置 jpa + H2 数据库持久化层

[Spring Data JPA(二)：SpringBoot集成H2](https://www.jianshu.com/p/d3d35975023f)， 摘要如下：

#### pom.xml (part: jpa + h2) 

```xml
<dependencies>
    ...
	<!-- 添加数据持久化 jpa 依赖，activiti 官方文档中推荐使用 jpa 集成 -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- 数据库 h2 -->
    <dependency>
      <groupId>com.h2database</groupId>
      <artifactId>h2</artifactId>
      <version>1.4.196</version>
    </dependency>
    ...
</dependencies>
```

#### application.yml (part: jpa + h2) 

```yml
spring:
  datasource:
  	# 创建了一个内存级的数据库 mem:h2test
    url: jdbc:h2:mem:h2test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
    platform: h2
    username: sa
    password:
    driverClassName: org.h2.Driver
  jpa:
    database-platform: org.hibernate.dialect.H2Dialect
    hibernate:
      ddl-auto: update
    properties:
      hibernate:
        show_sql: true
        use_sql_comments: true
        format_sql: true
  h2:
    console:
      enabled: true
      # 项目启动以后，使用 http://localhost:8080/h2 访问 H2 控制台
      path: /h2
      settings:
        trace: false
        web-allow-others: false
```

### 配置 activtivi

#### pom.xml (part: activiti)

引入 [Activiti Spring Boot Starter Basic](https://mvnrepository.com/artifact/org.activiti/activiti-spring-boot-starter-basic) 6.0.0 maven 依赖

```xml
<dependencies>
    ...
    <!-- activiti 依赖 -->
    <dependency>
        <groupId>org.activiti</groupId>
        <artifactId>activiti-spring-boot-starter-basic</artifactId>
        <version>6.0.0</version>
    </dependency>
    ...
</dependencies>
```

#### application.yml (part: activiti)

首次启动的时候，碰到这个问题：activiti 中自动搜索流程文件的 check-process-definitions 问题

```
2019-07-09 14:49:01.400 ERROR 23687 --- [           main] o.s.boot.SpringApplication               : Application startup failed

org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'springProcessEngineConfiguration' defined in class path resource [org/activiti/spring/boot/JpaProcessEngineAutoConfiguration$JpaConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.activiti.spring.SpringProcessEngineConfiguration]: Factory method 'springProcessEngineConfiguration' threw exception; nested exception is java.io.FileNotFoundException: class path resource [processes/] cannot be resolved to URL because it does not exist
```

[spring boot2集成activiti6的问题记录](https://www.jianshu.com/p/59b3f079cd75) 的最后部分，介绍了这个问题的解决办法

```yml
spring:
	activiti:
		# 每一次 spring boot 启动的时候，重新初始化数据库
		database-schema-update: true
		# 不检查 processes/ 目录下是否有流程
    	check-process-definitions: false
```

### pom.xml (all)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.21.RELEASE</version>
    <relativePath /> <!-- lookup parent from repository -->
  </parent>
  <groupId>com.wdzggroup.rbzy</groupId>
  <artifactId>oa-base</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <name>oa-base</name>
  <description>oa-base project for Spring Boot</description>

  <properties>
    <java.version>1.8</java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- activiti 依赖 -->
    <dependency>
      <groupId>org.activiti</groupId>
      <artifactId>activiti-spring-boot-starter-basic</artifactId>
      <version>6.0.0</version>
    </dependency>

    <!-- 添加数据持久化 jpa 依赖，activiti 官方文档中推荐使用 jpa 集成 -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- 数据库 h2 -->
    <dependency>
      <groupId>com.h2database</groupId>
      <artifactId>h2</artifactId>
      <version>1.4.196</version>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>

</project>

```

### application.yml (all)

```yml
spring:
  datasource:
    url: jdbc:h2:mem:h2test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
    platform: h2
    username: sa
    password:
    driverClassName: org.h2.Driver
  jpa:
    database-platform: org.hibernate.dialect.H2Dialect
    hibernate:
      ddl-auto: update
    properties:
      hibernate:
        show_sql: true
        use_sql_comments: true
        format_sql: true
  h2:
    console:
      enabled: true
      path: /h2
      settings:
        trace: false
        web-allow-others: false
  activiti:
      database-schema-update: true
      check-process-definitions: false
```