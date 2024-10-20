# Java 12 的新特性是什么

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/what-is-new-in-java-12/>

![Java 12 logo](img/96e330d54ef57b30b57d586bcdccbf3d.png)

[Java 12](http://web.archive.org/web/20221124003700/https://openjdk.java.net/projects/jdk/12/) 于 2019 年 3 月 19 日正式发布，[在此下载 Java 12](http://web.archive.org/web/20221124003700/https://jdk.java.net/java-se-ri/12)或此 [openJDK 存档](http://web.archive.org/web/20221124003700/https://jdk.java.net/archive/)。

Java 12 特性。

*   [1。JEP 189: Shenandoah:一个低停顿时间的垃圾收集器(实验)](#jep-189-shenandoah-a-low-pause-time-garbage-collector-experimental)
*   [2。JEP 230:微基准测试套件](#jep-230-microbenchmark-suite)
*   [3。JEP 325:切换表情(预览)](#jep-325-switch-expressions-preview)
*   [4。JEP 334: JVM 常量 API](#jep-334-jvm-constants-api)
*   [5。JEP 340:一个 AArch64 端口，而不是两个](#jep-340-one-aarch64-port-not-two)
*   [6。JEP 341:默认 CDS 档案](#jep-341-default-cds-archives)
*   [7。JEP 344:G1 可接受的混合收藏](#jep-344-abortable-mixed-collections-for-g1)
*   [8。JEP 346:及时从 G1 返回未使用的提交内存](#jep-346-promptly-return-unused-committed-memory-from-g1)

*Java 12 开发者特性。*

切换表达式(预览)

## 1。JEP 189: Shenandoah:一个低停顿时间的垃圾收集器(实验)

Shenandoah 是一个新的低暂停和并发垃圾收集器，阅读这篇[研究论文](http://web.archive.org/web/20221124003700/https://www.researchgate.net/publication/306112816_Shenandoah_An_open-source_concurrent_compacting_garbage_collector_for_OpenJDK)，它减少了 GC 暂停时间并且独立于 Java 堆大小(5M 或 5G 的堆大小有相同的暂停时间，对大型堆应用有用。)

这个 GC 是一个实验性的特性，我们需要使用以下选项来启用新的 Shenandoah GC。

```java
 -XX:+UnlockExperimentalVMOptions -XX:+UseShenandoahGC 
```

然而， [Oracle JDK 和 OpenJDK 都不包含这个新的 Shenandoah GC](http://web.archive.org/web/20221124003700/http://mail.openjdk.java.net/pipermail/shenandoah-dev/2018-December/008673.html) ，也请阅读这个[并非所有 OpenJDK 12 版本都包含 Shenandoah:下面是原因](http://web.archive.org/web/20221124003700/https://developers.redhat.com/blog/2019/04/19/not-all-openjdk-12-builds-include-shenandoah-heres-why/)。

```java
 C:\Users\mkyong> java -version
java version "12" 2019-03-19
Java(TM) SE Runtime Environment (build 12+33)
Java HotSpot(TM) 64-Bit Server VM (build 12+33, mixed mode, sharing)

C:\Users\mkyong> java -XX:+UnlockExperimentalVMOptions -XX:+UseShenandoahGC
Error occurred during initialization of VM
Option -XX:+UseShenandoahGC not supported 
```

为了尝试 Shenandoah GC，我们需要其他 JDK 版本，如 [AdoptOpenJDK](http://web.archive.org/web/20221124003700/https://adoptopenjdk.net/) 。

这个 Shenandoah GC 成为了 Java 15 [JEP 379](http://web.archive.org/web/20221124003700/https://openjdk.java.net/jeps/379) 的一个产品特性。

*延伸阅读*

*   [JEP 189:谢南多厄:一个低停顿时间的垃圾收集器(实验)](http://web.archive.org/web/20221124003700/https://openjdk.java.net/jeps/189)
*   [Shenandoah OpenJDK 首页](http://web.archive.org/web/20221124003700/https://openjdk.java.net/projects/shenandoah/)

## 2。JEP 230:微基准测试套件

向 JDK 源代码添加了一系列 Java 微基准测试工具(JMH ),对于那些有兴趣添加或修改 JDK 源代码本身的人来说，现在他们有了一种比较性能的方法。

**注**
抱歉，不知道它是如何工作的，如果你知道如何运行 JDK 的基准测试，请在下面评论。

*延伸阅读*

*   [JEP 230:微基准测试套件](http://web.archive.org/web/20221124003700/https://openjdk.java.net/jeps/230)

## 3。JEP 325:切换表情(预览)

这个 JEP 增强了现有的 switch 语句(不返回任何内容)以支持 switch 表达式(返回某些内容)。

传统的 switch 语句，我们可以通过将值赋给变量来返回值:

```java
 private static String getText(int number) {
        String result = "";
        switch (number) {
            case 1, 2:
                result = "one or two";
                break;
            case 3:
                result = "three";
                break;
            case 4, 5, 6:
                result = "four or five or six";
                break;
            default:
                result = "unknown";
                break;
        };
        return result;
    } 
```

在 Java 12 中，我们可以使用`break`或`case L ->`从开关返回值。

```java
 private static String getText(int number) {
        String result = switch (number) {
            case 1, 2:
                break "one or two";
            case 3:
                break "three";
            case 4, 5, 6:
                break "four or five or six";
            default:
                break "unknown";
        };
        return result;
    } 
```

`case L ->`语法。

```java
 private static String getText(int number) {
        return switch (number) {
            case 1, 2 -> "one or two";
            case 3 -> "three";
            case 4, 5, 6 -> "four or five or six";
            default -> "unknown";
        };
    } 
```

**注意**
这个 switch 表达式在 Java 13 中有了第二个预览版(去掉了`break`取而代之的是`yield`)，并且这个 switch 表达式在 [Java 14](http://web.archive.org/web/20221124003700/https://mkyong.com/java/what-is-new-in-java-14/) 中成为了一个标准特性。

*进一步阅读*

*   [JEP 325:开关表情(预览)](http://web.archive.org/web/20221124003700/https://openjdk.java.net/jeps/325)
*   [Java 12 开关表达式](http://web.archive.org/web/20221124003700/https://mkyong.com/java/java-12-switch-expressions/)

要启用 Java 12 预览功能:

```java
 javac --enable-preview --release 12 Example.java
java --enable-preview Example 
```

## 4。JEP 334: JVM 常量 API

一个新的包`java.lang.constant`，一系列新的类和接口来建模关键的类文件和运行时工件，例如[常量池](http://web.archive.org/web/20221124003700/https://docs.oracle.com/javase/specs/jvms/se10/html/jvms-4.html#jvms-4.4)。

*延伸阅读*

*   JEP 334: JVM 常量 API
*   [包`java.lang.constant`](http://web.archive.org/web/20221124003700/https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/constant/package-summary.html)

## 5。JEP 340:一个 AArch64 端口，而不是两个

在 Java 12 之前，64 位 [ARM 架构](http://web.archive.org/web/20221124003700/https://en.wikipedia.org/wiki/ARM_architecture)有两种不同的源代码或端口。

*   甲骨文—`src/hotspot/cpu/arm`
*   red hat？`src/hotspot/cpu/aarch64`

Java 12 移除了 Oracle `src/hotspot/cpu/arm`端口，只保留了一个端口`src/hotspot/cpu/aarch64`，并让这个`aarch64`成为 64 位 ARM 架构的默认构建。

*延伸阅读*

*   [JEP 340:一个 AArch64 端口，而不是两个](http://web.archive.org/web/20221124003700/https://openjdk.java.net/jeps/340)

## 6。JEP 341:默认 CDS 档案

[类数据共享(CDS)](http://web.archive.org/web/20221124003700/https://docs.oracle.com/javase/8/docs/technotes/guides/vm/class-data-sharing.html) 通过重用现有的归档文件来缩短启动时间。

在 Java 12 之前，我们需要使用`-Xshare: dump`为 JDK 类生成 CDS 归档文件。在 Java 12 中，`/bin/server/`目录中有一个新的`classes.jsa`文件，这是 JDK 类的默认 CDS 归档文件。

![default cds](img/e3cfca4ff60744b751f31786b4464325.png)

*延伸阅读*

*   [JEP 341:默认 CDS 档案](http://web.archive.org/web/20221124003700/https://openjdk.java.net/jeps/341)

## 7。JEP 344:G1 可接受的混合收藏

这个 JEP 通过将有问题的收集组分成两部分——强制的和可选的，提高了垃圾优先(G1)收集器的性能。如果没有足够的时间来处理可选部分，G1 将中止它。

*延伸阅读*

*   [JEP 344:G1 可接受的混合收藏](http://web.archive.org/web/20221124003700/https://openjdk.java.net/jeps/334)

## 8。JEP 346:及时从 G1 返回未使用的提交内存

这个 JEP 提高了垃圾优先(G1)收集器的性能。如果应用程序活动较少或空闲，G1 会定期触发一个并发周期来确定总体 Java 堆使用情况，并将未使用的 Java 堆内存返回给操作系统。

*延伸阅读*

*   [JEP 346:及时从 G1 返回未使用的提交内存](http://web.archive.org/web/20221124003700/https://openjdk.java.net/jeps/346)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221124003700/https://github.com/mkyong/core-java)

$ cd java-12

## 参考文献

*   [OpenJDK 12 项目](http://web.archive.org/web/20221124003700/https://openjdk.java.net/projects/jdk/12/)
*   [Java 版本历史](http://web.archive.org/web/20221124003700/https://en.wikipedia.org/wiki/Java_version_history)
*   [Shenandoah OpenJDK 首页](http://web.archive.org/web/20221124003700/https://openjdk.java.net/projects/shenandoah/)
*   [Shenandoah 研究论文](http://web.archive.org/web/20221124003700/https://www.researchgate.net/publication/306112816_Shenandoah_An_open-source_concurrent_compacting_garbage_collector_for_OpenJDK)
*   [Java 12 开关表达式](http://web.archive.org/web/20221124003700/https://mkyong.com/java/java-12-switch-expressions/)
*   [Java 13 开关表达式](http://web.archive.org/web/20221124003700/https://www.mkyong.com/java/java-13-switch-expressions/)
*   [Java 微基准测试工具(JMH)](http://web.archive.org/web/20221124003700/https://openjdk.java.net/projects/code-tools/jmh/)
*   [java.lang.constant](http://web.archive.org/web/20221124003700/https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/constant/package-summary.html)
*   [ARM 架构](http://web.archive.org/web/20221124003700/https://en.wikipedia.org/wiki/ARM_architecture)
*   [类数据共享](http://web.archive.org/web/20221124003700/https://docs.oracle.com/javase/8/docs/technotes/guides/vm/class-data-sharing.html)
*   [JDK 发布 39 项新功能(和 API)12](http://web.archive.org/web/20221124003700/https://www.azul.com/39-new-features-and-apis-in-jdk-12/)
*   [维基百科——垃圾优先收集器](http://web.archive.org/web/20221124003700/https://en.wikipedia.org/wiki/Garbage-first_collector)
*   [开始使用 G1 垃圾收集器](http://web.archive.org/web/20221124003700/https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html)
*   Java 安全性有什么新特性？

<input type="hidden" id="mkyong-current-postId" value="15738">