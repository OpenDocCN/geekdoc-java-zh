# Java–流已经被操作或关闭

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-stream-has-already-been-operated-upon-or-closed/>

在 Java 8 中，[流](http://web.archive.org/web/20221221112126/https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)不能被重用，一旦被消耗或使用，该流将被关闭。

## 1.示例–流已关闭！

回顾下面的例子，它会抛出一个`IllegalStateException`，说“stream closed”。

TestJava8.java

```java
 package com.mkyong.java8;

import java.util.Arrays;
import java.util.stream.Stream;

public class TestJava8 {

    public static void main(String[] args) {

        String[] array = {"a", "b", "c", "d", "e"};
        Stream<String> stream = Arrays.stream(array);

        // loop a stream
        stream.forEach(x -> System.out.println(x));

        // reuse it to filter again! throws IllegalStateException
        long count = stream.filter(x -> "b".equals(x)).count();
        System.out.println(count);

    }

} 
```

输出

```java
java.lang.IllegalStateException: stream has already been operated upon or closed
	at java.util.stream.AbstractPipeline.(AbstractPipeline.java:203)
	at java.util.stream.ReferencePipeline.<init>(ReferencePipeline.java:94)
	at java.util.stream.ReferencePipeline$StatelessOp.<init>(ReferencePipeline.java:618)
	at java.util.stream.ReferencePipeline$2.<init>(ReferencePipeline.java:163)
	at java.util.stream.ReferencePipeline.filter(ReferencePipeline.java:162)
	at com.hostingcompass.whois.range.run.TestJava8.main(TestJava8.java:25)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144) 
```

## 2.示例–重用流

不管出于什么原因，你真的想重用一个流，尝试下面的`Supplier`解决方案:

TestJava8.java

```java
 package com.mkyong.java8;

import java.util.function.Supplier;
import java.util.stream.Stream;

public class TestJava8 {

    public static void main(String[] args) {

        String[] array = {"a", "b", "c", "d", "e"};

        Supplier<Stream<String>> streamSupplier = () -> Stream.of(array);

        //get new stream
        streamSupplier.get().forEach(x -> System.out.println(x));

        //get another new stream
        long count = streamSupplier.get().filter(x -> "b".equals(x)).count();
        System.out.println(count);

    }

} 
```

输出

```java
a
b
c
d
e
1

```

每个`get()`将返回一个新的流。

**Why?**
May I know why you need to reuse a stream, for testing?

## 参考

1.  [供应商 JavaDoc](http://web.archive.org/web/20221221112126/https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html)
2.  [流摘要 JavaDoc](http://web.archive.org/web/20221221112126/https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)

<input type="hidden" id="mkyong-current-postId" value="13971">