# 使用RestTemplate发送请求

[TOC]

# 参考资料

[使用RestTemplate发送get请求,获取不到参数的问题](https://blog.csdn.net/zzzgd_666/article/details/82791180)

> ```java
> UriComponentsBuilder builder = UriComponentsBuilder.fromHttpUrl(url);
>     String url2 = builder.queryParam("name", "zgd").build().encode().toString();
> ```
>
> 

[Spring RestTemplate GET with parameters](https://stackoverflow.com/questions/8297215/spring-resttemplate-get-with-parameters)

> ```java
> HttpHeaders headers = new HttpHeaders();
> headers.set("Accept", MediaType.APPLICATION_JSON_VALUE);
> 
> UriComponentsBuilder builder = UriComponentsBuilder.fromHttpUrl(url)
>         .queryParam("msisdn", msisdn)
>         .queryParam("email", email)
>         .queryParam("clientVersion", clientVersion)
>         .queryParam("clientType", clientType)
>         .queryParam("issuerName", issuerName)
>         .queryParam("applicationName", applicationName);
> 
> HttpEntity<?> entity = new HttpEntity<>(headers);
> 
> HttpEntity<String> response = restTemplate.exchange(
>         builder.toUriString(), 
>         HttpMethod.GET, 
>         entity, 
>         String.class);
> ```

[Springboot -- 用更优雅的方式发HTTP请求(RestTemplate详解)](https://www.jianshu.com/p/27a82c494413)

> exchange()方法跟上面的getForObject()、getForEntity()、postForObject()、postForEntity()等方法不同之处在于它可以指定请求的HTTP类型。

[RestTemplate使用教程](https://www.cnblogs.com/f-anything/p/10084215.html)

> 文章排版不好，但是说到一个类 RestTemplateBuilder



# 返回类型为集合时如何处理

[RestTemplate请求的服务实例返回List类型，用数组接收](https://blog.csdn.net/horse_well/article/details/88879185)

> 把服务端代码改为数组接收，在将数组转化为List

[RestTemplate解决返回类型为Object<List<T>>问题](https://my.oschina.net/u/2246525/blog/1583457)

> ```java
> ResponseEntity<Result<List<TreeNode>>> response = restTemplate.exchange(targetUrl + "?userId=" + userId, 
>               HttpMethod.GET, 
>               entity, 
>               new ParameterizedTypeReference<Result<List<TreeNode>>>(){});
> ```
>
> 

# 下划线转驼峰问题处理

[RestTemplate＆Jackson - 自定义JSON反序列化？](http://cn.voidcc.com/question/p-dqqszppw-bdx.html)

> 这里说到一个关键，可以通过定制 AbstractHttpMessageConverter 实现 RestTemplate 下划线转驼峰的问题
>
> 查看 AbstractHttpMessageConverter 发现 fastJson 有一个实现 FastJsonHttpMessageConverter

[How to set PropertyNamingStrategy for RestTemplate in SpringBoot?](https://stackoverflow.com/questions/36095844/how-to-set-propertynamingstrategy-for-resttemplate-in-springboot)

> 中文版：[如何在SpringBoot中为RestTemplate设置PropertyNamingStrategy？](https://codeday.me/bug/20190519/1135482.html)

# RestTemplate 参数说明

[RestTemplate uriVariables 怎么使用详解](https://blog.csdn.net/qq_26878363/article/details/96161133)