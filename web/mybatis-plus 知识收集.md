# mybatis-plus 知识收集

[TOC]

# 参考资料

[mybatis-plus id主键生成的坑](https://blog.csdn.net/qq_34208844/article/details/88819467)

> 解决问题：
>
> 由于`mybatis-plus`会自动插入一个`id`到实体对象, 不管你封装与否, 所以有时候导致一些意外的情况发生：默认是生成一个长数字字符串(编码不同可能结尾带有字母)
>
> ```
> @TableId(value = "id",type = IdType.AUTO)
> ```

