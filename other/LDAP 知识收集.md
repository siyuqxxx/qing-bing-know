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

[LDIF修改ldap记录或配置示例](http://seanlook.com/2015/01/22/openldap_ldif_example/) 引用的文章很给力，英文的


## 缩略词字典

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

