# java 异常链处理

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

