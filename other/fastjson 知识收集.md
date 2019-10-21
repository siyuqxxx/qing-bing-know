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

