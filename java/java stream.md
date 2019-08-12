# java stream

[TOC]

# 参考资料

## list -> map 存在重复值的处理方法

[java8 toMap(key重复如何解决)](https://blog.csdn.net/qq_32002237/article/details/78580254)

> Collectors.toMap(xxx, xxx, (a,b) -> a)

[Java8 stream操作toMap的key重复问题](https://my.oschina.net/u/3725073/blog/1807970/)

> ```java
> userList.stream().collect(Collectors.toMap(User::getId,
>                 e -> Stream.of(e.getUsername()).collect(Collectors.toList()),
>                 (List<String> oldList, List<String> newList) -> {
>                     oldList.addAll(newList);
>                     return oldList;
>                 }));
> ```

[Java8新特性之Collectors](https://blog.csdn.net/u013291394/article/details/52662761)

> toMap(Task::getTitle, task -> task)
>
> 改进 
>
> toMap(Task::getTitle, identity())

[lambda-为什么要boxed](https://blog.csdn.net/u014041227/article/details/70544949)

> 报错的
>
> ```java
>  IntStream.range(0, 10).collect(Collectors.toList());
> ```
>
> 正确的
>
> ```java
>  IntStream.range(0,10).boxed().collect(Collectors.toList());
> ```

[java8 stream流操作的flatMap（流的扁平化）](https://blog.csdn.net/Mark_Chao/article/details/80810030)

> 案例：对给定单词列表 ["Hello","World"],你想返回列表["H","e","l","o","W","r","d"]
>
> ```java
> String[] words = new String[]{"Hello","World"};
> List<String> a = Arrays.stream(words)
>     .map(word -> word.split(""))
>     .flatMap(Arrays::stream)
>     .distinct()
>     .collect(toList());
> ```

[Java8 Stream使用flatMap合并List](https://blog.csdn.net/weixin_41835612/article/details/83713891)

> ```java
> /*交集*/
>     /*[AClass(id=1, name=zhuoli1, description=haha1)]*/
>     List<AClass> intersectResult = aClassList1.stream().filter(aClassList2::contains).collect(Collectors.toList());
>     System.out.println(intersectResult);
> 
> /*并集*/
> List<AClass> unionResult = Stream.of(aClassList1, aClassList2).flatMap(Collection::stream).distinct().collect(Collectors.toList());
> assertEquals(unionResult.size(), 5);
> System.out.println(unionResult);
>  
> /*差集*/
> /*[AClass(id=2, name=zhuoli2, description=haha2), AClass(id=3, name=zhuoli3, description=haha3)]*/
> List<AClass> differenceResult = aClassList1.stream().filter(x -> !aClassList2.contains(x)).collect(Collectors.toList());
> System.out.println(differenceResult);
> ```
>