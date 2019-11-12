# fastjson 知识收集

[TOC]

[Fastjson关于泛型的 json 转 对象](https://blog.csdn.net/u011418943/article/details/84289556)

> ```JAVA
> BaseResponse<HotPotInfo> response =  JSON.parseObject(s, new TypeReference<BaseResponse<HotPotInfo>>(){});
> ```
>
> 

[fastjson生成json时Null属性不显示的解决方法](https://www.jianshu.com/p/a3bf11b9a3a3)

> ```java
> @JSONField(serialzeFeatures= {SerializerFeature.WriteMapNullValue})
> ```
>
> 

[FastJson解析对象中的泛型](https://blog.csdn.net/yoyohoho1/article/details/91364185)

[基于FastJson的通用泛型解决方案](https://www.cnblogs.com/scy251147/p/9451879.html)

> ```java
> //开启fastjson autotype功能（不开启，造成EntityWrapper<T>中的T无法正常解析）
> ParserConfig.getGlobalInstance().setAutoTypeSupport(true);
> ```

[fastjson 1.2.61 发布，增加 autoType 安全黑名单](https://www.oschina.net/news/110001/fastjson-1-2-61-released)

