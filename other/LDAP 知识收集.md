# LDAP 知识收集

[TOC]

## 参考资料

[我花了一个五一终于搞懂了OpenLDAP](https://segmentfault.com/a/1190000014683418)

> 可以这样讲：市面上只要你能够想像得到的所有工具软件，全部都支持`LDAP`协议。比如说你公司要安装一个项目管理工具，那么这个工具几乎必然支持`LDAP`协议，你公司要安装一个`bug`管理工具，这工具必然也支持`LDAP`协议，你公司要安装一套软件版本管理工具，这工具也必然支持`LDAP`协议。`LDAP`协议的好处就是你公司的所有员工在所有这些工具里共享同一套用户名和密码，来人的时候新增一个用户就能自动访问所有系统，走人的时候一键删除就取消了他对所有系统的访问权限，这就是`LDAP`。

[LDAP](https://www.jianshu.com/p/7e4d99f6baaf) - 这篇文章介绍了什么是 LDAP ，以及 LDAP 数据存储的基本逻辑

[LDAP统一账号密码管理工具搭建配置](https://www.jianshu.com/p/55cb4dcc8757) - 有一种看起来很厉害的感觉，介绍了一个完善的案例，后续再做研究

[LDAP 调研](https://www.jianshu.com/p/8aef9e08a394)

> 包含一个 **人员部门目录结构设计(最佳实例)** ，见下图
>
> ![img](https://upload-images.jianshu.io/upload_images/9026439-8ce177678734fea5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/849/format/webp)
> ![4e977490ce1b09e8f6ba7f6be2eadbae.png](en-resource://database/869:1)

[OpenLDAP 概念与工作原理介绍](https://www.jianshu.com/p/a3f8190e189c)

> 介绍了 2 种OpenLDAP 目录架构，分别用 2 张图描述了
>
> + 互联网命名组织架构
>
> + 企业级命名组织架构

[LDAP概念与原理](https://www.jianshu.com/p/deae0356a1df) - 提供了一个目录结构设计的案例

> 这里面有一个 objectClass 的概念，不知道是什么鬼

[OpenLDAP Software Administrator's Guides](http://www.openldap.org/doc) - 2.X 版本的官方手册

[LDAP服务器的概念和原理简单介绍](http://seanlook.com/2015/01/15/openldap_introduction/) - 可惜好多图片加载不出来，因此，查看下面这篇博客

[LDAP基础概念](https://blog.51cto.com/407711169/1439623) - 突然找到了很多图片的出处... **好文！** 

> 对象类（ObjectClass）是属性的集合
>
> 
>
> 每个条目 (Entry) 可以直接继承多个对象类，这样就继承了各种属性。
>
> 
>
> 对象类有三种类型：`结构类型 (Structural)`、`抽象类型 (Abstract)` 和`辅助类型 (Auxiliary)`。
>
> + `结构类型 (Structural)` 是最基本的类型，它规定了对象尸体的基本属性，每个条目属于且仅属于一个结构型对象类。
> + `抽象类型(Abstract)` 可以是结构类型或其他抽象类型父类，它公国将对象属性中共性的部分组织在一起，称为其他类的模板，条目不能直接集成抽象型对象类。
> + `辅助类型(Auxiliary)` 规定了对象实体的扩展属性。
> + 虽然每个`条目 (Entry)`只属于一个`结构类型 (Structural)`，但可以同时属于多个`辅助类型(Auxiliary)`。

[运维与LDAP（openldap）](https://blog.51cto.com/407711169/1439944) - 这是一个目录，包含了上面的这篇文章，以及一些其他的文章

[LDAP Linux HOWTO](http://www.tldp.org/HOWTO/LDAP-HOWTO/index.html) - 可以可以，然而是英文的

[LDAP使用说明文档](https://blog.csdn.net/u013452335/article/details/81280655)

> 官方文档中说，使用的是 sladp 作为数据库
>
> 这篇文章中说，使用了 PostgreSQL 作为数据库

[OpenLDAP：用ACL控制访问权限](http://blog.sina.com.cn/s/blog_4152a9f50100qw9w.html) - ACL 访问控制概述

[OPENLDAP 访问控制](https://blog.csdn.net/xubo578/article/details/5170868) 很细腻，很接近官方文档

[zTree根据LDAP树形结构数据展示到页面](https://my.oschina.net/u/3010328/blog/827346)  - 一个编码方面的介绍

[几种常见的LDAP系统](https://www.lijiaocn.com/技巧/2017/04/01/ldap.html) - 以用的

[rfc4510 LDAP: Technical Specification Road Map, June 2006](http://www.rfc-editor.org/info/rfc4510)

[rfc4511 LDAP: The Protocol, June 2006](http://www.rfc-editor.org/info/rfc4511)

[rfc4512 LDAP: Directory Information Models, June 2006](http://www.rfc-editor.org/info/rfc4512)

[rfc4513 LDAP: Authentication Methods and Security Mechanisms, June 2006](http://www.rfc-editor.org/info/rfc4513)

[rfc4514 LDAP: String Representation of Distinguished Names](http://www.rfc-editor.org/info/rfc4514)

[rfc4515 LDAP: String Representation of Search Filters](http://www.rfc-editor.org/info/rfc4515)

[rfc4516 LDAP: Uniform Resource Locator](http://www.rfc-editor.org/info/rfc4516)

[rfc4517 LDAP: Syntaxes and Matching Rules](http://www.rfc-editor.org/info/rfc4517)

[rfc4518 LDAP: Internationalized String Preparation](http://www.rfc-editor.org/info/rfc4518)

[rfc4519 LDAP: Schema for User Applications](http://www.rfc-editor.org/info/rfc4519) 这个标准 (RFC-4195) 准确的定义了各种 ldap 中的所略语

[LDIF修改ldap记录或配置示例](http://seanlook.com/2015/01/22/openldap_ldif_example/) 引用的文章可以，but 英文的

[LDAP服务器中的rootDSE概念](http://blog.chinaunix.net/uid-20503928-id-3577591.html)

> DSA=Directory System Agent, 翻译过来就是目录系统代理
>
> DSE=DSA Specific Entry， 翻译过来就是目录系统中的特定条目或者记录项，是一个本地目录服务器的控制条目。它不是LDAP命名空间的一部分，rootDSE的目的就是提供关于directory Server的本身的数据。每个Directory Server都有自己的rootDSE信息，是不完全一致的。一般来说rootDSE的信息都是对所有用可读的。
>
> rootDSE包含下面的一些信息:
>
>
> + 厂商/供应商=Vendor
> + 服务器支持的命名上下文=naming contexts
> + 服务器支持的请求控制=request control
> + 支持的SASL机制
> + 支持的功能
> + Schema位置等等信息

[OpenLDAP2.4管理员指南]([http://wiki.jabbercn.org/index.php/OpenLDAP2.4%E7%AE%A1%E7%90%86%E5%91%98%E6%8C%87%E5%8D%97#Syncrepl.E4.BB.A3.E7.90.86](http://wiki.jabbercn.org/index.php/OpenLDAP2.4管理员指南#Syncrepl.E4.BB.A3.E7.90.86))

[在LDAP目录树中怎么组织数据](http://blog.sina.com.cn/s/blog_5384e78b0100eyg6.html)

[设计LDAP目录树](https://blog.csdn.net/liuzh501448/article/details/1572844)

[Linux/UNIX OpenLDAP实战指南 (郭大勇著)完整pdf扫描版[97MB]](https://www.jb51.net/books/584400.html) - 下载码：7pmx

[请教：如何用LDAP实现软件的操作权限管理](http://bbs.chinaunix.net/thread-498490-1-1.html)

[APACHE + LDAP 的权限认证配置方法(转)](https://wise007.iteye.com/blog/203172)

> 一个不错的方案，关键摘要如下：
>
> 对用户通过“组(groups)”进行管理，对于需要权限控制的目录， 则通过“组”进行控制。

[OpenLDAP 图形化管理](https://blog.51cto.com/wzlinux/1837363)

[OpenLDAP限制用户登录主机](https://blog.51cto.com/fengwan/1846879)

[nginx 引入LDAP验证](https://blog.51cto.com/duyunlong/1896137)

## 缩略词字典

核心参考资料：[rfc4519 LDAP: Schema for User Applications](http://www.rfc-editor.org/info/rfc4519) 这个标准 (RFC-4195) 准确的定义了各种 ldap 中的所略语

| 层级  | 属性 | 全名                         | 描述            | 备注                                 |
| ----- | ---- | ---------------------------- | --------------- | ------------------------------------ |
| 1     | c    | country Name                 | 国家            |                                      |
| 2     | o    | organization                 | 组织 / 公司     |                                      |
| 2     | dc   | domain Component             | 域名            | (哪一颗树)                           |
| 3     | ou   | organizationalUnitName       | 组织单元 / 部门 | (哪一个分支)                         |
| 4     | sn   | surname                      | 真实名称        | 必要属性                             |
| 4     | cn   | commonName                   | 常用名称        | 必要属性                             |
|       | uid  | userid                       |                 | 用户id                               |
|       |      |                              |                 |                                      |
| other | dn   | Distinguished Name           | 唯一标识        | 描述具体条目所处树形结构的**全路径** |
| other | rdn  | relative distinguished names | 相对唯一表示    | **每一段路径**                       |

## LDAP 客户端

| 名称            | 类型          | 备注                                                         |
| --------------- | ------------- | ------------------------------------------------------------ |
| phpLDAPadmin    | web           |                                                              |
| LDAP admin tool | windows linux | [windows](http://www.ldapbrowserwindows.com/) \| [linux](http://www.ldapbrowserlinux.com/) |
| JXplorer        | windows linux | [官网](http://www.jxplorer.org/)                             |

