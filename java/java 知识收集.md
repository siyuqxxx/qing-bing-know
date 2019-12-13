

# java 知识收集

[TOC]

# @NotNull

[由@NotNull注解引出的关于Java空指针的控制](http://www.cppcns.com/ruanjian/java/165323.html)

[@NotNull和@NonNull区别和使用](https://www.jianshu.com/p/32327ca2365f)

[Bean Validation](https://beanvalidation.org/) - 官方文档

[使用Bean Validation实现数据校验](https://www.jianshu.com/p/2a495bf5504e)

# lambda 表达式 与 设计模式

[Java8 in action(1) 通过行为参数化传递代码--lambda代替策略模式](https://yq.aliyun.com/articles/442018)

[从策略模式闲扯到lambda表达式](https://www.jqhtml.com/39192.html)

# JVM 参数

[JVM 参数](https://blog.csdn.net/liyongbing1122/article/details/88716400)

[jvm参数汇总](https://blog.csdn.net/qq_35901087/article/details/89361803)

[JVM - 参数简介](https://www.jianshu.com/p/1c6b5c2e95f9)

> 排版比较清晰

[SpringBoot项目优化和Jvm调优(楼主亲测，真实有效)](https://www.cnblogs.com/jpfss/p/9753215.html)

> 图文并茂，包含一些实验数据和截图

[Java 堆内存 新生代 （转）](https://www.cnblogs.com/junwangzhe/p/6282550.html)

> 对内存的介绍非常清晰，摘要如下
>
> 
>
> **Java** 中的堆也是 GC 收集垃圾的主要区域。GC 分为两种：Minor GC、Full GC ( 或称为 Major GC )。
> Minor GC 是发生在新生代中的垃圾收集动作，所采用的是复制算法。
> 新生代几乎是所有 **Java** 对象出生的地方，即 **Java** 对象申请的内存以及存放都是在这个地方。**Java** 中的大部分对象通常不需长久存活，具有朝生夕灭的性质。
> 当一个对象被判定为 "死亡" 的时候，GC 就有责任来回收掉这部分对象的内存空间。新生代是 GC 收集垃圾的频繁区域。
> 当对象在 Eden ( 包括一个 Survivor 区域，这里假设是 from 区域 ) 出生后，在经过一次 Minor GC 后，如果对象还存活，并且能够被另外一块 Survivor 区域所容纳
> ( 上面已经假设为 from 区域，这里应为 to 区域，即 to 区域有足够的内存空间来存储 Eden 和 from 区域中存活的对象 )，则使用复制算法将这些仍然还存活的对象复制到另外一块 Survivor 区域 ( 即 to 区域 ) 中，然后清理所使用过的 Eden 以及 Survivor 区域 ( 即 from 区域 )，并且将这些对象的年龄设置为1，以后对象在 Survivor 区每熬过一次 Minor GC，就将对象的年龄 + 1，当对象的年龄达到某个值时 ( 默认是 15 岁，可以通过参数 -XX:MaxTenuringThreshold 来设定 )，这些对象就会成为老年代。
> 但这也不是一定的，对于一些较大的对象 ( 即需要分配一块较大的连续内存空间 ) 则是直接进入到老年代。
> Full GC 是发生在老年代的垃圾收集动作，所采用的是标记-清除算法。
> 现实的生活中，老年代的人通常会比新生代的人 "早死"。堆内存中的老年代(Old)不同于这个，老年代里面的对象几乎个个都是在 Survivor 区域中熬过来的，它们是不会那么容易就 "死掉" 了的。因此，Full GC 发生的次数不会有 Minor GC 那么频繁，并且做一次 Full GC 要比进行一次 Minor GC 的时间更长。
> 另外，标记-清除算法收集垃圾的时候会产生许多的内存碎片 ( 即不连续的内存空间 )，此后需要为较大的对象分配内存空间时，若无法找到足够的连续的内存空间，就会提前触发一次 GC 的收集动作。

[JVM参数调优-设置堆、新生代、老年代、持久代大小](https://blog.csdn.net/qq_30509055/article/details/93670031)

> 这篇文章中讲了如何计算虚拟机参数，提供了案例和计算方法