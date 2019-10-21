# MySQL 知识收集

[TOC]

[MySQL 使用自增ID主键和UUID 作为主键的优劣比较详细过程（从百万到千万表记录测试）](https://www.cnblogs.com/barrywxx/p/7723122.html)

>1. 单实例或者单节点组
>
>   经过500W、1000W的单机表测试，自增ID相对UUID来说，自增ID主键性能高于UUID，磁盘存储费用比UUID节省一半的钱。所以在单实例上或者单节点组上，使用自增ID作为首选主键。
>
>2. 分布式架构场景
>   + 20个节点组下的小型规模的分布式场景，为了快速实现部署，可以采用多花存储费用、牺牲部分性能而使用UUID主键快速部署；
>   + 20到200个节点组的中等规模的分布式场景，可以采用自增ID+步长的较快速方案。
>   + 200以上节点组的大数据下的分布式场景，可以借鉴类似twitter雪花算法构造的全局自增ID作为主键。  

# 远程 数据库 表 同步

[MYSQL数据库间同步数据](https://blog.csdn.net/zdagf/article/details/80441524)

> 关注 binlog 进行数据库同步

[MySQL触发器实现两表数据同步（详解）](https://blog.csdn.net/qq_21891743/article/details/85061495)

[mysql 建立远程同步表](https://blog.csdn.net/qq_38083665/article/details/80720116)

# 数据库设计规范

[值得收藏：一份非常完整的MySQL规范](https://zhuanlan.zhihu.com/p/69773290)

# SQL JOIN 相关知识

[[译文\] SQL JOIN，你想知道的应该都有](https://www.cnblogs.com/xufeiyang/p/5818571.html)

![img](https://www.codeproject.com/KB/database/Visual_SQL_Joins/Visual_SQL_JOINS_V2.png)