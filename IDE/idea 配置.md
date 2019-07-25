# idea 配置

[TOC]

# 编辑器配置

## 显示空白字符

editor - general - Appearance - `Show whitespaces`

## 多行显示 Tab 页签

editor - general
+ editor tabs - `Show tabs in one row`(去勾选) 
+ tab limit - 20

## 字体配置

editor - font - `Courier New`

## xml 用 空格 替换 tab

editor - Code Style - xml 

`Tab size`: 2
`Indent`: 2
`Continuation indent`: 4

## 取消注释到行首

editor - Code Style - java / html / xml
+ Code Generation
  + （去勾选）Line comment at first column
    + （勾选 | 仅 java 涉及）Add a sapce at comment start
  + （去勾选）Block comment at first column

# 快捷键设置

## 在文件浏览器中打开(show in explorer)

keymap - show in explorer - ctrl + alt + \

# 变量颜色

Editor - Color Scheme - java
+ Parameters
  + Parameter 
    + Foreground - RGB: 7F5F00
    + Bold - 勾选
  + Reassigned Parameter
    + Foreground - RGB: 7F5F00
    + Bold - 勾选
+ variables
  + Reassigned Local variable
    + Foreground - RGB: 7F5F00
  + Local variable
    + Foreground - RGB: 7F5F00
  
# 插件

## mybatis

free mybatis plugin

## restful 测试工具

RestfulToolkit

个人觉得没有 postman 好用

## lombok

lombok

# 专题配置

## spring boot 支持 热部署

[IntelliJ IDEA Spring boot devtools 实现热部署](https://www.cnblogs.com/zxguan/p/7941711.html)

[Springboot在idea中使用devtools热部署配置不生效的解决办法](https://blog.csdn.net/sinat_25305411/article/details/81030503)

### 修改 pom.xml 

增加 springboot devtools 依赖，并配置插件，开启热部署

```xml
<dependencies>
    ...
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
    ...
</dependencies>

<build>
    <plugins>
        ...
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
            </configuration>
        </plugin>
        ...
    </plugins>
</build>
```

### 配置 idea

+ `ctrl`+`alt`+`s` 设置
  + Build, Execution, Deployment - Compiler
    + （勾选）Build project automatically 

+ 按快捷键Ctrl+Shift+Alt+/，选择1.Registry...
  + （勾选）compiler.automake.allow.when.app.running
+ 重启 idea

# 问题排除

## tomcat 中文乱码

[IntelliJ IDEA运行tomcat项目中文乱码问题](https://blog.csdn.net/u011877584/article/details/80368102)