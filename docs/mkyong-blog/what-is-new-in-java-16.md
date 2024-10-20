# Java 16 的新特性是什么

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/what-is-new-in-java-16/>

![Java 16 logo](img/cdeecfe3aafadd386816cbc526eed129.png)

[Java 16](http://web.archive.org/web/20221225035521/https://openjdk.java.net/projects/jdk/16/) 于 2021 年 3 月 16 日正式上市，[点击此处](http://web.archive.org/web/20221225035521/https://jdk.java.net/16/)下载 Java 16。

Java 16 有 17 个 JEP 项目。

*   [1。JEP 338:载体 API(孵化器)](#jep-338-vector-api-incubator)
*   [2。JEP 347:启用 C++14 语言特性](#jep-347enable-c14-language-features)
*   [3。JEP 357:从 Mercurial 迁移到 Git](#jep-357-migrate-from-mercurial-to-git)
*   [4。JEP 369:迁移到 GitHub](#jep-369-migrate-to-github)
*   [5。JEP 376: ZGC:并发线程堆栈处理](#jep-376-zgc-concurrent-thread-stack-processing)
*   [6。JEP 380: Unix 域套接字通道](#jep-380-unix-domain-socket-channels)
*   [7。JEP 386:阿尔卑斯 Linux 端口](#jep-386-alpine-linux-port)
*   [8。JEP 387:弹性元空间](#jep-387-elastic-metaspace)
*   [9。jep 388:windows/aach 64 端口](#jep-388-windowsaarch64-port)
*   10。JEP 389:外来链接器 API(孵化器)
*   [11。JEP 390:基于价值的类的警告](#jep-390-warnings-for-value-based-classes)
*   [12。JEP 392:包装工具](#jep-392-packaging-tool)
*   13。JEP 393:外部内存访问 API(第三个孵化器)
*   [14。JEP 394:实例的模式匹配](#jep-394-pattern-matching-for-instanceof)
*   15。JEP 395:记录
*   16。JEP 396:默认强封装 JDK 内部构件
*   [17。JEP 397:密封类(第二次预览)](#jep-397-sealed-classes-second-preview)
*   [下载源代码](#download-source-code)
*   [参考文献](#references)

*Java 16 开发者特性。*

jpackage、记录(标准)、模式匹配(标准)、密封类型(第二次预览)、外部内存访问 API(第三个孵化器)、替换 JNI 的外部链接器 API(孵化器)、vector APIs(孵化器)

## 1。JEP 338:载体 API(孵化器)

Java 支持[自动向量化](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Automatic_vectorization)来优化算术算法，这意味着 Java ( [JIT 编译器](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Just-in-time_compilation))会自动将一些标量操作(一次一项)转化为向量操作(一次多项)；然而，开发人员无法控制这种向量操作转换，它完全依赖于 JIT 编译器来优化代码，而且，并不是所有的标量操作都是可转换的。

这个 JEP 引入了新的 Vector APIs，允许开发人员显式地执行 Vector 操作。

*   一个[向量处理器](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Vector_processor)一次处理多个数据，称为[单指令多数据(SIMD)](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/SIMD) 。
*   一个[标量处理器](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Scalar_processor)一次只处理一个数据，称为[单指令单数据(SISD)](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/SISD) 。

**延伸阅读**

*   [JEP 338:载体 API(培养箱)](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/338)
*   [Java 中的自动矢量化](http://web.archive.org/web/20221225035521/http://daniel-strecker.com/blog/2020-01-14_auto_vectorization_in_java/)
*   [Java 17 JEP 414: Vector API(第二孵化器)](http://web.archive.org/web/20221225035521/https://mkyong.com/java/what-is-new-in-java-17/#jep-414-vector-api-second-incubator)

## 2。JEP 347:启用 C++14 语言特性

这个 JEP 允许在 JDK 内的 C++源代码中使用[C++ 14 特性]((https://en . Wikipedia . org/wiki/C % 2B % 2b 14)。

**延伸阅读**

*   [JEP 347:启用 C++14 语言特性](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/347)

## 3。JEP 357:从 Mercurial 迁移到 Git

本 JEP 将 OpenJDK 源代码从 Mercurial 迁移到 Git 或 GitHub，下面涉及到 [JEP 369](#)

迁移到 Git 的原因:

1.  版本控制系统元数据(Mercurial)文件太大。
2.  可用工具
3.  可用主机

**延伸阅读**

*   [JEP 357:从 Mercurial 迁移到 Git](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/357)

## 4。JEP 369:迁移到 GitHub

此次 JEP 加入上述 [JEP 357](#jep-357-migrate-from-mercurial-to-git) ，将 OpenJDK 源代码从 Mercurial 迁移到 [GitHub](http://web.archive.org/web/20221225035521/https://github.com/openjdk/) 。

**延伸阅读**

*   [JEP 369:迁移到 GitHub](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/369)

## 5。JEP 376: ZGC:并发线程堆栈处理

这个 JEP 通过将 ZGC 线程堆栈处理从安全点转移到并发阶段，改进了 Z 垃圾收集器(ZGC)。

*历史*

*   Java 11 [JEP 333](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/333) 引入了 Z 垃圾收集器(ZGC)作为实验性的垃圾收集器。
*   Java 15 [JEP 377](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/377) ，ZGC 成为了产品的一大特色。

**延伸阅读**

*   JEP 376: ZGC:并发线程堆栈处理

## 6。JEP 380: Unix 域套接字通道

[Unix 域套接字](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Unix_domain_socket)用于同一主机上的进程间通信(IPC ),这意味着在同一主机上执行的进程之间交换数据。Unix 域套接字类似于 TCP/IP 套接字，只是它们由文件系统路径名寻址，而不是由互联网协议(IP)地址和端口号寻址。大多数 Unix 平台，Windows 10 和 Windows Server 2019，也支持 Unix 域套接字。

此次 JEP 在现有的 [SocketChannel](http://web.archive.org/web/20221225035521/https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/nio/channels/SocketChannel.html) 和 [ServerSocketChannel](http://web.archive.org/web/20221225035521/https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/nio/channels/ServerSocketChannel.html) 的基础上增加了[Unix-domain(AF _ Unix)socket](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Unix_domain_socket)支持。

新的 Unix 域套接字类或 API:

*   新套接字地址类，`java.net.UnixDomainSocketAddress`
*   新增`enum`，`java.net.StandardProtocolFamily.UNIX`

**延伸阅读**

*   [JEP 380: Unix 域套接字通道](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/380)

## 7。JEP 386:阿尔卑斯 Linux 端口

这个 JEP 把 JDK 移植到了 Alpine Linux 和其他使用 musl 实现的 Linux 发行版上。这个 JDK 端口使 Java 能够在 Alpine Linux 中开箱即用，这有利于那些依赖于 Java 的框架或工具，如 Tomcat 和 Spring。

Alpine Linux 包含较小的映像大小，广泛用于云部署、微服务和容器环境。

**延伸阅读**

*   [JEP 386:阿尔卑斯 Linux 端口](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/386)

## 8。JEP 387:弹性元空间

Java 8 [JEP 122](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/122) 移除了 PermGen(永久生成)，并引入了[元空间](http://web.archive.org/web/20221225035521/https://wiki.openjdk.java.net/display/HotSpot/Metaspace)，这是 hotspot 中的一个原生堆外内存管理器。

该 JEP 通过更迅速地将未使用的热点类元数据或元空间内存返回给操作系统，减少了元空间占用空间，并简化了元空间代码，从而改进了元空间内存管理。

**延伸阅读**

*   JEP 387:弹性元空间

## 9。jep 388:windows/aach 64 端口

该 JEP 将 JDK 移植到 Windows/AArch64，在 ARM 硬件、服务器或基于 ARM 的笔记本电脑上运行 JDK + Windows。

Windows/AArch64 是最终用户市场的普遍需求。

**延伸阅读**

*   [jep 388:windows/aach 64 端口](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/388)

## 10。JEP 389:外来链接器 API(孵化器)

该 JEP 使 Java 代码能够调用或被用其他语言编写的本机代码调用，如 C 或 C++，取代 [Java 本机接口(JNI)](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Java_Native_Interface)

这是一个孵化的特征；需要添加`--add-modules jdk.incubator.foreign`来编译和运行 Java 代码。

下面的例子显示了如何使用外部链接器 API 来调用标准 C 库`strlen`以返回字符串的长度。

```java
 size_t strlen(const char *s); 
```

CStrLen.java

```java
 import jdk.incubator.foreign.*;

import java.lang.invoke.MethodHandle;
import java.lang.invoke.MethodType;

import static jdk.incubator.foreign.CLinker.C_LONG;
import static jdk.incubator.foreign.CLinker.C_POINTER;

// Java call standard C library
// size_t strlen(const char *s);
public class CStrLen {

  public static void main(String[] args) throws Throwable {

      if (args.length == 0) {
          throw new IllegalArgumentException("Please provide an argument.");
      }

      String input = args[0];

      MethodHandle strlen = CLinker.getInstance().downcallHandle(
              LibraryLookup.ofDefault().lookup("strlen").get(),
              MethodType.methodType(long.class, MemoryAddress.class),
              FunctionDescriptor.of(C_LONG, C_POINTER)
      );

      try (MemorySegment str = CLinker.toCString(input)) {
          long len = (long) strlen.invokeExact(str.address()); // 5
          System.out.println(len);
      }

  }
} 
```

启用孵化器模块进行编译。

Terminal

```java
 $ javac --add-modules jdk.incubator.foreign CStrLen.java

warning: using incubating module(s): jdk.incubator.foreign
1 warning 
```

启用保育箱模块运行。

Terminal

```java
 $ java --add-modules jdk.incubator.foreign -Dforeign.restricted=permit CStrLen mkyong

WARNING: Using incubator modules: jdk.incubator.foreign
6 
```

下面的 10.2 是另一个调用 C 代码中定义的函数的例子。

打印 hello world 的简单 C 函数。

```java
 #include <stdio.h>

void printHello() {
        printf("hello world!\n");
} 
```

编译上面的 C 代码，输出到共享库`hello.so`。

Terminal

```java
 $ gcc -c -fPIC hello.c
$ gcc -shared -o hello.so hello.o 
```

在 Java 程序下面，找到`hello.so`并调用它的方法`printHello`。

JEP389.java

```java
 import jdk.incubator.foreign.CLinker;
import jdk.incubator.foreign.FunctionDescriptor;
import jdk.incubator.foreign.LibraryLookup;

import java.lang.invoke.MethodHandle;
import java.lang.invoke.MethodType;
import java.nio.file.Path;
import java.util.Optional;

public class JEP389 {

  public static void main(String[] args) throws Throwable {

      Path path = Path.of("/home/mkyong/projects/core-java/java-16/hello.so");

      LibraryLookup libraryLookup = LibraryLookup.ofPath(path);

      Optional<LibraryLookup.Symbol> optionalSymbol = libraryLookup.lookup("printHello");
      if (optionalSymbol.isPresent()) {

          LibraryLookup.Symbol symbol = optionalSymbol.get();

          FunctionDescriptor functionDescriptor = FunctionDescriptor.ofVoid();

          MethodType methodType = MethodType.methodType(Void.TYPE);

          MethodHandle methodHandle = CLinker.getInstance().downcallHandle(
                  symbol.address(),
                  methodType,
                  functionDescriptor);
          methodHandle.invokeExact();

      }

  }
} 
```

编译。

Terminal

```java
 $ javac --add-modules jdk.incubator.foreign JEP389.java

warning: using incubating module(s): jdk.incubator.foreign
1 warning 
```

快跑。

Terminal

```java
 $ java --add-modules jdk.incubator.foreign -Dforeign.restricted=permit JEP389

WARNING: Using incubator modules: jdk.incubator.foreign
hello world! 
```

*历史*

*   Java 14 [JEP 370](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/370) 引入了外来内存访问 API(孵化器)。
*   Java 15 [JEP 383](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/383) 引入了外来内存访问 API(第二孵化器)。
*   Java 16 [JEP 389](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/389) 引入了国外的链接器 API(孵化器)。
*   Java 16 [JEP 393](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/393) 引入了外来内存访问 API(第三孵化器)。
*   Java 17 [JEP 412](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/412) 引入了外来函数&内存 API(孵化器)。

**延伸阅读**

*   [JEP 389:外来链接器 API(孵化器)](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/389)

## 11。JEP 390:基于价值的类的警告

如果我们同步[基于值的类](http://web.archive.org/web/20221225035521/https://download.java.net/java/early_access/jdk16/docs/api/java.base/java/lang/doc-files/ValueBased.html)的实例，这个 JEP 提供了一个新的警告；也不赞成移除原始包装类(基于值的)构造函数。

11.1 如何识别基于价值的类？
注释`@jdk.internal.ValueBased`告诉我们一个类是否是基于值的类。回顾下面的两个类，我们可以看出原始包装类`Double`和`Optional`都是基于值的类。

Optional.java

```java
 package java.util;

@jdk.internal.ValueBased
public final class Optional<T> {
  //...
} 
```

Double.java

```java
 package java.lang;

@jdk.internal.ValueBased
public final class Double extends Number
        implements Comparable<Double>, Constable, ConstantDesc {

  //...
} 
```

**注**
参考 [JEP 390:基于价值类的警告](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/390)了解基于价值类的完整列表。

11.2 下面的例子尝试`synchronized`一个基于值的类。

JEP390.java

```java
 public class JEP390 {

    public static void main(String[] args) {

        Double d = 20.0;
        synchronized (d) {} // javac warning & HotSpot warning
    }
} 
```

编译上面的代码，点击新的警告。

Terminal

```java
 $ javac JEP390.java
JEP390.java:7: warning: [synchronization] attempt to synchronize on an instance of a value-based class
        synchronized (d) {} // javac warning & HotSpot warning
        ^
1 warning 
```

11.3 Java 9 弃用了原始包装类(基于值的)构造函数，现已标记为移除。

Double.java

```java
 package java.lang;

//...

@jdk.internal.ValueBased
public final class Double extends Number
        implements Comparable<Double>, Constable, ConstantDesc {

  @Deprecated(since="9", forRemoval = true)
  public Double(double value) {
      this.value = value;
  }

  @Deprecated(since="9", forRemoval = true)
  public Double(String s) throws NumberFormatException {
      value = parseDouble(s);
  }

  //...
} 
```

**延伸阅读**

*   [JEP 390:对基于价值的类的警告](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/390)

这是关于未来版本中的值类型吗？

*   [基于值的类](http://web.archive.org/web/20221225035521/https://nipafx.dev/java-value-based-classes/)
*   [Java 中的新值类型](http://web.archive.org/web/20221225035521/https://jaxenter.com/java-value-type-163446.html)

## 12。JEP 392:包装工具

JEP 将`jpackage`工具从`jdk.incubator.jpackage`移至`jdk.jpackage`，并成为 Java 16 中的标准或产品特性。这个`jpackage`是一个打包工具，用于将 Java 应用打包成特定于平台的包，例如:

*   Linux: deb 和 rpm
*   macOS: pkg 和 dmg
*   Windows: msi 和 exe

*历史*

*   Java 14 [JEP 343](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/343) 引入了一个`jpackage`孵化工具，它在 Java 15 中仍然是一个孵化工具。

12.1 下面的例子展示了在 Linux 系统(Ubuntu)中使用`jpackage`将一个简单的 Java Swing 程序打包成`deb`格式。

一个简单的 Java Swing 程序来显示 hello world。

JEP392.java

```java
 import javax.swing.*;
import java.awt.*;

public class JEP392 {

    public static void main(String[] args) {

        JFrame frame = new JFrame("Hello World Java Swing");
        // display frame site
        frame.setMinimumSize(new Dimension(800, 600));
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // center the JLabel
        JLabel lblText = new JLabel("Hello World!", SwingConstants.CENTER);
        // add JLabel to JFrame
        frame.getContentPane().add(lblText);

        // display it
        frame.pack();
        frame.setVisible(true);

    }
} 
```

创建一个`jar`文件并将它`jpackage`到一个特定于平台的包中；因为这是在 Ubuntu 系统中测试的，所以它会创建一个`.deb`文件。

Terminal

```java
 # compile
$ javac JEP392.java

# create a jar file
$ jar cvf hello.jar JEP392.class

# package the jar file into platform-specific package
$ /opt/jdk-16/bin/jpackage -i . -n JEP392 --main-jar hello.jar --main-class JEP392

# The jpackage created this jep392_1.0-1_amd64.deb
$ ls -lsah
4.0K -rw-rw-r--  1 mkyong mkyong  994 Mac  15 13:52 hello.jar
 30M -rw-r--r--  1 mkyong mkyong  30M Mac  15 14:01 jep392_1.0-1_amd64.deb 
```

[安装。deb 文件](http://web.archive.org/web/20221225035521/https://unix.stackexchange.com/questions/159094/how-to-install-a-deb-file-by-dpkg-i-or-by-apt)；

Terminal

```java
 sudo dpkg -i jep392_1.0-1_amd64.deb
Selecting previously unselected package jep392.
(Reading database ... 225576 files and directories currently installed.)
Preparing to unpack jep392_1.0-1_amd64.deb ...
Unpacking jep392 (1.0-1) ...
Setting up jep392 (1.0-1) ...

$ ls -lsah /opt/jep392/bin/JEP392
1.6M -rwxr-xr-x 1 root root 1.6M Mac  15 14:01 /opt/jep392/bin/JEP392 
```

上的默认安装目录

*   Linux 是`/opt`
*   macOS 是`/Applications`
*   Windows 是`C:\Program Files\`

*P.S 这可以通过`jpackage --install-dir`选项覆盖。*

运行已安装的 Java Swing 程序。

Terminal

```java
 $ /opt/jep392/bin/JEP392 
```

输出

![hello world swing](img/4c423581bc484eb8fd4e68d94cf270f4.png)

`jpackage -h`永远是你最好的朋友:

Terminal

```java
 $ /opt/jdk-16/bin/jpackage -h
Usage: jpackage <options>

Sample usages:
--------------
  Generate an application package suitable for the host system:
      For a modular application:
          jpackage -n name -p modulePath -m moduleName/className
      For a non-modular application:
          jpackage -i inputDir -n name \
              --main-class className --main-jar myJar.jar
      From a pre-built application image:
          jpackage -n name --app-image appImageDir
  Generate an application image:
      For a modular application:
          jpackage --type app-image -n name -p modulePath \
              -m moduleName/className
      For a non-modular application:
          jpackage --type app-image -i inputDir -n name \
              --main-class className --main-jar myJar.jar
      To provide your own options to jlink, run jlink separately:
          jlink --output appRuntimeImage -p modulePath -m moduleName \
              --no-header-files [<additional jlink options>...]
          jpackage --type app-image -n name \
              -m moduleName/className --runtime-image appRuntimeImage
  Generate a Java runtime package:
      jpackage -n name --runtime-image <runtime-image>

  //... 
```

**延伸阅读**

*   [JEP 392:包装工具](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/392)

## 13。JEP 393:外部内存访问 API(第三个孵化器)

外来内存访问 API 允许 Java API 访问 Java 堆外的外来内存，如 [memcached](http://web.archive.org/web/20221225035521/https://github.com/dustin/java-memcached-client) 、 [Lucene](http://web.archive.org/web/20221225035521/https://lucene.apache.org/) 等。

这个 JEP 更新了外部内存访问 API，并保留了[孵化器模块](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/11)。

*历史*

*   Java 14 [JEP 370](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/370) 引入了外来内存访问 API(孵化器)。
*   Java 15 [JEP 383](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/383) 引入了外来内存访问 API(第二孵化器)。
*   Java 16 [JEP 389](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/389) 引入了国外的链接器 API(孵化器)。
*   Java 16 [JEP 393](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/393) 引入了外来内存访问 API(第三孵化器)。
*   Java 17 [JEP 412](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/412) 引入了外来函数&内存 API(孵化器)。

**延伸阅读**

*   [JEP 393:外来存储器访问 API(第三孵化器)](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/393)

## 14。JEP 394:实例的模式匹配

`instanceof`的模式匹配是 Java 16 中的标准或产品特性。

在模式匹配之前，我们检查对象的类型并手动转换为变量。

```java
 if (obj instanceof String) {
    String s = (String) obj;    // cast
} 
```

现在我们可以检查对象的类型并自动转换它

```java
 if (obj instanceof String s) {
    //... s is a string
} 
```

例如，下面是一个常见的检查和强制转换示例。

```java
 if (obj instanceof String) {
      String s = (String) obj;
      if (s.length() > 5) {
          if (s.equalsIgnoreCase("java16")) {
              //...
          }
      }
  } 
```

并且，我们可以使用新的`instanceof`重构上面的代码。

```java
 if (obj instanceof String s && s.length() > 5) {
      if (s.equalsIgnoreCase("java16")) {
          //...
      }
  } 
```

*历史*

*   Java 14 [JEP 305](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/305) ，首次预告。
*   Java 15 [JEP 375](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/375) ，第二次预告。
*   Java 16，标准特性。

**延伸阅读**

*   [JEP 394:实例的模式匹配](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/394)
*   [Java 17 JEP 406:开关的模式匹配(预览)](http://web.archive.org/web/20221225035521/https://mkyong.com/java/what-is-new-in-java-17/#jep-406-pattern-matching-for-switch-preview)

## 15。JEP 395:记录

`record`最终确定并成为标准特征。

JEP395.java

```java
 package com.mkyong.java16.jep395;

public class JEP395 {

    record Point(int x, int y) { }

    public static void main(String[] args) {

        Point p1 = new Point(10, 20);
        System.out.println(p1.x());         // 10
        System.out.println(p1.y());         // 20

        Point p2 = new Point(11, 22);
        System.out.println(p2.x());         // 11
        System.out.println(p2.y());         // 22

        Point p3 = new Point(10, 20);
        System.out.println(p3.x());         // 10
        System.out.println(p3.y());         // 20

        System.out.println(p1.hashCode());  // 330
        System.out.println(p2.hashCode());  // 363
        System.out.println(p3.hashCode());  // 330

        System.out.println(p1.equals(p2));  // false
        System.out.println(p1.equals(p3));  // true
        System.out.println(p2.equals(p3));  // false

    }
} 
```

*历史*

*   Java 14 [JEP 359](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/359) ，首次预告。
*   Java 15 [JEP 384](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/384) ，第二次预告。
*   Java 16，标准特性。

**延伸阅读**

*   [JEP 395:记录](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/395)

## 16。JEP 396:默认强封装 JDK 内部构件

Java 9 [JEP 261](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/261#Relaxed-strong-encapsulation) 引入了`--illegal-access`选项来控制对内部 API 的访问，并打包成 JDK。

该 JEP 将`--illegal-access`选项的默认模式从允许改为拒绝。随着这一改变，JDK 的内部包和 API(除了[关键内部 API](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/260#Description))将不再默认打开。

这个 JEP 的动机是阻止第三方库、框架和工具使用 JDK 的内部 API 和包。

**延伸阅读**

*   [JEP 396:默认强封装 JDK 内部构件](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/396)
*   Java 17 JEP 403:强封装 JDK 内部

## 17。JEP 397:密封类(第二次预览)

Java 15 [JEP 360](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/360) 引入了[密封类和接口](http://web.archive.org/web/20221225035521/https://cr.openjdk.java.net/~briangoetz/amber/datum.html)来限制哪个类可以扩展或实现它们。这个 JEP 是第二个预览版，有一些改进。

**延伸阅读**

*   [JEP 397:密封类(第二次预览)](http://web.archive.org/web/20221225035521/https://openjdk.java.net/jeps/397)

这个密封类是 Java 17 中的一个标准特性。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221225035521/https://github.com/mkyong/core-java)

$ cd java-16

## 参考文献

*   [OpenJDK 16 项目](http://web.archive.org/web/20221225035521/https://openjdk.java.net/projects/jdk/16/)
*   [OpenJDK 源代码](http://web.archive.org/web/20221225035521/https://github.com/openjdk/)
*   [维基百科–自动矢量化](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Automatic_vectorization)
*   [Java 中的自动矢量化](http://web.archive.org/web/20221225035521/http://daniel-strecker.com/blog/2020-01-14_auto_vectorization_in_java/)
*   [矢量处理器](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Vector_processor)
*   [标量处理器](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Scalar_processor)
*   [C++14 的语言特性](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/C%2B%2B14)
*   [维基百科–Unix 域套接字](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Unix_domain_socket)
*   [IntelliJ IDEA 2021.1 EAP 1:支持 Java 16](http://web.archive.org/web/20221225035521/https://blog.jetbrains.com/idea/2021/01/intellij-idea-2021-1-eap-1/)
*   [open JDK–什么是元空间](http://web.archive.org/web/20221225035521/https://wiki.openjdk.java.net/display/HotSpot/Metaspace)
*   [基于值的类](http://web.archive.org/web/20221225035521/https://download.java.net/java/early_access/jdk16/docs/api/java.base/java/lang/doc-files/ValueBased.html)
*   【Java 的数据类和密封类型
*   【Java 的模式匹配
*   [open JDK Wiki–ZCG](http://web.archive.org/web/20221225035521/https://wiki.openjdk.java.net/display/zgc/Main)
*   [Java 版本历史](http://web.archive.org/web/20221225035521/https://en.wikipedia.org/wiki/Java_version_history)

<input type="hidden" id="mkyong-current-postId" value="16646">