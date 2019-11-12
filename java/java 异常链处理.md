# java 异常链处理

[TOC]



# 思想

异常处理的核心是制造异常、传递异常、处理异常

# 异常链处理算法

```java
public static final String EXCEPTION_CHAIN_SEPARATOR = " cause by: ";

private static String getErrMsgFromExceptionChain(Exception e, boolean needSeparator) {
    List<String> errMsgList = new LinkedList<>();

    for(Throwable cause = e; cause != null; cause = cause.getCause()) {
        errMsgList.add(cause.getMessage());
    }

    return String.join(needSeparator ? EXCEPTION_CHAIN_SEPARATOR : "", errMsgList);
}
```

