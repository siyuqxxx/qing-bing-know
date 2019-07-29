# lombok 知识收集

[TOC]

# 参考资料

[Project Lombok](https://projectlombok.org/)  - lombok 的官方网址

>  里面有一个 lombok 三分四十九秒的视频讲解，概要的介绍了，lombok 是弄啥的

[IntelliJ IDEA lombok插件的安装和使用](https://jingyan.baidu.com/article/0a52e3f4e53ca1bf63ed725c.html)

[lombok—@Accessors注解](http://www.tianwenjie.cn/lombok-accessorszhu-jie/)

[@EqualsAndHashCode](https://www.jianshu.com/p/70fa9b64b652)

[lombok @Slf4j注解](https://blog.csdn.net/xue632777974/article/details/80437452)

> ```java
> import lombok.extern.slf4j.Slf4j;
> 
> @Slf4j
> public class LogExample {
> }
> ```
>
> 以上将编译成
>
> ```java
> public class LogExample {
>  private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(LogExample.class);
> }
> ```
>
> 