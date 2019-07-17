# spring boot mysql

[TOC]

[spring-boot中初始化sql讲解及分析](https://blog.csdn.net/yinbucheng/article/details/80164395) 这篇讲解了源码，说明，springboot 时如何通过配置文件初始化数据到数据库中的

springboot 如何连接 mysql 数据库

[[六]SpringBoot 之 连接数据库(mybatis)](https://www.cnblogs.com/s648667069/p/6483341.html)


src/main/resource/application.properties
```conf
spring.datasource.url = jdbc:mysql://123.206.228.200:3306/test
spring.datasource.username = shijunjie
spring.datasource.password = ******
```

# 使用 mybatis generator


官方网站：[Running MyBatis Generator With Maven](http://www.mybatis.org/generator/running/runningWithMaven.html)

mysql schema catlog 区别 [MySql中的schema和catalog](https://blog.csdn.net/l153097889/article/details/66475376?utm_source=blogxgwz3)

这篇文章讲了，如何使得 mybatis generator 自动 生成的文件可以覆盖原来的文件

[springboot用mybatis-generator自动生成mapper和model](https://www.cnblogs.com/fengli9998/p/7689103.html)


[MyBatis Generator（MBG）](https://www.jianshu.com/p/d0abe91d50b0)

## pom.xml

```xml
<build>
  ...
  <plugins>
    ...
    <plugin>
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-maven-plugin</artifactId>
      <version>1.3.7</version>
      <configuration>
        <verbose>true</verbose>
        <overwrite>true</overwrite>
        <configurationFile>${basedir}/src/main/resources/mybatis-generator/mybatis-generator.xml</configurationFile>
      </configuration>
      <dependencies>
        <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.16</version>
        </dependency>
      </dependencies>
    </plugin>
    ...
  </plugins>
  ...
</build>
```

## mybatis-generator.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
    PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
    "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
  <context id="DB2Tables">
    <commentGenerator>
      <property name="addRemarkComments" value="true"/>
      <property name="suppressDate" value="true"/>
      <property name="suppressAllComments" value="true"/>
    </commentGenerator>

    <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                    connectionURL="jdbc:mysql://mysql.qsy-rbzy.com:3306/dqmall"
                    userId="rbzy"
                    password="Rbzy123456">
    </jdbcConnection>

    <!--生成Model类存放位置-->
    <javaModelGenerator targetPackage="com.wdzggroup.rbzy.activiti.oa.pojo" targetProject="src/main/java">
      <property name="trimStrings" value="true"/>
    </javaModelGenerator>

    <!--生成映射文件存放位置-->
    <sqlMapGenerator targetPackage="com.wdzggroup.rbzy.activiti.oa.mapper" targetProject="src/main/resources">
    </sqlMapGenerator>

    <!--生成Dao类存放位置-->
    <javaClientGenerator type="XMLMAPPER" targetPackage="com.wdzggroup.rbzy.activiti.oa.dao" targetProject="src/main/java">
    </javaClientGenerator>

    <!--生成对应表及类名-->
    <table tableName="user" catalog="dqmall" enableCountByExample="true" enableUpdateByExample="true"
           enableDeleteByExample="true" enableSelectByExample="true" selectByExampleQueryId="true"/>
  </context>
</generatorConfiguration>
```

# 配置

## 数据库连接池

[数据库连接池性能测试(hikariCP,druid,tomcat-jdbc,dbcp,c3p0)](https://my.oschina.net/jzgycq/blog/1607039)  这个文章中介绍了几个数据库连接池的性能，功能

[SpringBoot+Mybatis+Druid的整合](https://blog.csdn.net/qq_36505948/article/details/82056017)

# 参数说明

## map-underscore-to-camel-case

下划线转驼峰

[Springboot中Mybatis属性映射](https://blog.csdn.net/zhao0416/article/details/78427191)

## default-fetch-size

不是很懂，先留存

[mysql的jdbc中fetchsize支持的问题](https://blog.csdn.net/gladmustang/article/details/41407851)

## default-statement-timeout

[Mybatis设置sql超时时间](https://blog.csdn.net/shuimuz_j/article/details/9674427)