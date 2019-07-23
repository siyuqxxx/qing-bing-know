# java stream

[TOC]

# 参考资料

[java8 toMap(key重复如何解决)](https://blog.csdn.net/qq_32002237/article/details/78580254)

> Collectors.toMap(xxx, xxx, (a,b) -> a)

[Java8新特性之Collectors](https://blog.csdn.net/u013291394/article/details/52662761)

> toMap(Task::getTitle, task -> task)
>
>  改进 
>
> toMap(Task::getTitle, identity())