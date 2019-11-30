# nginx 资料收集

[TOC]

# 参考资料

# nginx.conf 参数解析

## worker_processes

[nginx worker_processes 配置](
https://www.cnblogs.com/aaron-agu/p/8003831.html) 觉得这篇文章很不错的描述了worker_processes 这个参数


## charset

[Module ngx_http_charset_module](http://nginx.org/en/docs/http/ngx_http_charset_module.html) - nginx
[ngx_http_charset_module 模块](http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_charset_module.html) - nginx 官网介绍，中文翻译

## rewrite_log

[Module ngx_http_rewrite_module](http://nginx.org/en/docs/http/ngx_http_rewrite_module.html) - nginx
[ngx_http_rewrite_module 模块](http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_rewrite_module.html) - nginx 官网介绍，中文翻译

## proxy_read_timeout

[Module ngx_http_proxy_module](http://nginx.org/en/docs/http/ngx_http_proxy_module.html) - nginx
[ngx_http_proxy_module 模块](http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_proxy_module.html) - nginx 官网介绍，中文翻译

## tcp_nopush

[Module ngx_http_core_module](http://nginx.org/en/docs/http/ngx_http_core_module.html) - nginx
[ngx_http_core_module 模块](http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_core_module.html) - nginx 官网介绍，中文翻译

[TCP_NODELAY 和 TCP_NOPUSH的解释](https://www.cnblogs.com/wajika/p/6573014.html)
[从一起丢包故障来谈谈 nginx 中的 tcp keep-alive](https://segmentfault.com/a/1190000018011857)

## gzip

[Nginx系列5：nginx服务器的Gzip压缩](https://www.jianshu.com/p/141d5175a181)

## expires

[nginx 利用expires来让客户端缓存不常改变的数据](https://blog.csdn.net/hjh15827475896/article/details/53434298)



##  **client_max_body_size** 

[ngx_http_core_module 模块](http://tengine.taobao.org/nginx_docs/cn/docs/http/ngx_http_core_module.html#client_max_body_size)