# spring 统一异常处理

[TOC]

# 参考资料

[@RestControllerAdvice作用及原理](https://www.cnblogs.com/UncleWang001/p/10949318.html)

> 在spring 3.2中，新增了@ControllerAdvice 注解，可以用于定义@ExceptionHandler、@InitBinder、@ModelAttribute，并应用到所有@RequestMapping中。
>
> 如果全部异常处理返回json，那么可以使用 @RestControllerAdvice 代替 @ControllerAdvice ，这样在方法上就可以不需要添加 @ResponseBody。

[restController,controller,controllerAdvice,restControllerAdvice](https://www.jianshu.com/p/8f96c2037f9b)

> 排版实在不友好，内容还可以