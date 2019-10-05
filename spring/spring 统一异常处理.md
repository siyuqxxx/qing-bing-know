# spring 统一异常处理

[TOC]

# 参考资料

[@RestControllerAdvice作用及原理](https://www.cnblogs.com/UncleWang001/p/10949318.html)

> 在spring 3.2中，新增了@ControllerAdvice 注解，可以用于定义@ExceptionHandler、@InitBinder、@ModelAttribute，并应用到所有@RequestMapping中。
>
> 如果全部异常处理返回json，那么可以使用 @RestControllerAdvice 代替 @ControllerAdvice ，这样在方法上就可以不需要添加 @ResponseBody。

[restController,controller,controllerAdvice,restControllerAdvice](https://www.jianshu.com/p/8f96c2037f9b)

> 排版实在不友好，内容还可以

# @Validated + 统一异常处理

[sping4.3+ hibernate-validator@Validated注解失效最终解决方法](https://my.oschina.net/u/3293842/blog/1922735)

[一起来学SpringBoot | 第十八篇：轻松搞定全局异常](http://www.spring4all.com/article/1211)

```java
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.util.Optional;
import java.util.stream.Collectors;

@Slf4j
@RestControllerAdvice
public class ApiExceptionHandler {
    /**
     * 全局参数校验
     */
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public HttpEntity<String> bindExceptionErrorHandler(MethodArgumentNotValidException e) {
        log.error(e.getMessage());
        log.debug(e.getMessage(), e);
        String errMsg = e.getBindingResult().getFieldErrors().stream()
                .map(err -> String.format("%s: %s", err.getField(), Optional.ofNullable(err.getDefaultMessage()).orElse("")))
                .collect(Collectors.joining("; "));
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(errMsg);
    }
}
```

