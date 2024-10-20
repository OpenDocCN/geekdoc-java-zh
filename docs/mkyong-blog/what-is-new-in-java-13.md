# Java 13 的新特性是什么

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/what-is-new-in-java-13/>

![Java 13 logo](img/97933e4d2b6bd29c2744d61e3ff2f47a.png)

[Java 13](http://web.archive.org/web/20221124003658/https://openjdk.java.net/projects/jdk/13/) 于 2019 年 9 月 17 日正式发布，[在此下载 Java 13](http://web.archive.org/web/20221124003658/https://jdk.java.net/java-se-ri/13)或此 [openJDK 存档](http://web.archive.org/web/20221124003658/https://jdk.java.net/archive/)。

Java 13 特性。

*   [1。JEP 350 动态光盘档案](#jep-350-dynamic-cds-archives)
*   [2。JEP 351 ZGC:取消未使用内存的提交](#jep-351-zgc-uncommit-unused-memory)
*   [3。JEP-353 重新实现传统套接字 API](#jep-353-reimplement-the-legacy-socket-api)
*   [4。JEP-354 开关表情(预览)](#jep-354-switch-expressions-preview)
*   [5。JEP-355 文本块(预览)](#jep-355-text-blocks-preview)

*Java 13 开发者特性。*

切换表达式(预览)、文本块或多行(预览)

## 1。JEP 350 动态光盘档案

Java 10 引入了 [JEP 310 应用类——数据共享](http://web.archive.org/web/20221124003658/https://openjdk.java.net/jeps/310)。这种 JEP 简化了创建光盘档案的过程。

该命令创建一个`.jar`的 CDS 存档文件。

```java
 $ java -XX:ArchiveClassesAtExit=hello.jsa -cp hello.jar Hello 
```

该命令对现有的 CDS 档案运行`.jar`。

```java
 $  bin/java -XX:SharedArchiveFile=hello.jsa -cp hello.jar Hello 
```

类数据共享(CDS)通过创建一次类数据档案并重用它来提高启动性能，这样 JVM 就不需要重新创建它了。

*延伸阅读*

*   [JEP 350 动态光盘档案](http://web.archive.org/web/20221124003658/https://openjdk.java.net/jeps/350)
*   [cl4cds](http://web.archive.org/web/20221124003658/https://simonis.github.io/cl4cds/)
*   [通过应用程序类数据共享缩短 Java 13 的启动时间](http://web.archive.org/web/20221124003658/https://blog.codefx.org/java/application-class-data-sharing/)

## 2。JEP 351 ZGC:取消未使用内存的提交

Java 11 推出了 [JEP 333: Z 垃圾收集器(实验)](http://web.archive.org/web/20221124003658/https://openjdk.java.net/jeps/333)；在清理堆内存时，它提供了短暂的暂停时间。然而，它没有将未使用的堆内存返回给操作系统，即使它长时间未使用。

这个 JEP 通过将未使用的堆内存返回给操作系统来增强 ZGC。

*延伸阅读*

*   JEP 351 ZGC:取消未使用内存的提交

## 3。JEP-353 重新实现传统套接字 API

`java.net.Socket`和`java.net.ServerSocket`的底层实现非常古老，可以追溯到 JDK 1.0，这是一个混合了遗留 Java 和 C 代码的版本，很难维护和调试。这个 JEP 为 Socket APIs 引入了新的底层实现，这是 Java 13 中的默认实现。

在 Java 13 之前，它使用`PlainSocketImpl`作为`SocketImpl`

ServerSocket.java

```java
 public class ServerSocket implements java.io.Closeable {

    /**
     * The implementation of this Socket.
     */
    private SocketImpl impl;

} 
```

Java 13 引入了一个新的`NioSocketImpl`类作为`PlainSocketImpl`的替代。然而，如果出现问题，我们仍然可以通过设置`jdk.net.usePlainSocketImpl`系统属性切换回旧的实现`PlainSocketImpl`。

回顾一个简单的套接字示例。

JEP353.java

```java
 import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class JEP353 {

    public static void main(String[] args) {

        try (ServerSocket serverSocket = new ServerSocket(8888)){

            boolean running = true;
            while(running){

                Socket clientSocket = serverSocket.accept();
                //do something with clientSocket
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
} 
```

在 Java 13 中，默认实现是`NioSocketImpl`

Terminal

```java
 D:\test>javac JEP353.java

D:\test>java JEP353

D:\test>java -XX:+TraceClassLoading JEP353  | findStr Socket

[0.040s][info   ][class,load] java.net.ServerSocket source: jrt:/java.base
[0.040s][info   ][class,load] jdk.internal.access.JavaNetSocketAccess source: jrt:/java.base
[0.040s][info   ][class,load] java.net.ServerSocket$1 source: jrt:/java.base
[0.040s][info   ][class,load] java.net.SocketOptions source: jrt:/java.base
[0.040s][info   ][class,load] java.net.SocketImpl source: jrt:/java.base
[0.044s][info   ][class,load] java.net.SocketImpl$$Lambda$1/0x0000000800ba0840 source: java.net.SocketImpl
[0.047s][info   ][class,load] sun.net.PlatformSocketImpl source: jrt:/java.base

[0.047s][info   ][class,load] sun.nio.ch.NioSocketImpl source: jrt:/java.base

[0.047s][info   ][class,load] sun.nio.ch.SocketDispatcher source: jrt:/java.base
[0.052s][info   ][class,load] java.net.SocketAddress source: jrt:/java.base
[0.052s][info   ][class,load] java.net.InetSocketAddress source: jrt:/java.base
[0.052s][info   ][class,load] java.net.InetSocketAddress$InetSocketAddressHolder source: jrt:/java.base
[0.053s][info   ][class,load] sun.net.ext.ExtendedSocketOptions source: jrt:/java.base
[0.053s][info   ][class,load] jdk.net.ExtendedSocketOptions source: jrt:/jdk.net
[0.053s][info   ][class,load] java.net.SocketOption source: jrt:/java.base
[0.053s][info   ][class,load] jdk.net.ExtendedSocketOptions$ExtSocketOption source: jrt:/jdk.net
[0.053s][info   ][class,load] jdk.net.SocketFlow source: jrt:/jdk.net
[0.053s][info   ][class,load] jdk.net.ExtendedSocketOptions$PlatformSocketOptions source: jrt:/jdk.net
[0.053s][info   ][class,load] jdk.net.ExtendedSocketOptions$PlatformSocketOptions$1 source: jrt:/jdk.net
[0.054s][info   ][class,load] jdk.net.ExtendedSocketOptions$1 source: jrt:/jdk.net
[0.054s][info   ][class,load] sun.nio.ch.NioSocketImpl$FileDescriptorCloser source: jrt:/java.base
[0.055s][info   ][class,load] java.net.Socket source: jrt:/java.base 
```

我们可以通过设置`Djdk.net.usePlainSocketImpl`系统属性切换回`PlainSocketImpl`。

Terminal

```java
 D:\test>java -Djdk.net.usePlainSocketImpl -XX:+TraceClassLoading JEP353  | findStr Socket

[0.041s][info   ][class,load] java.net.ServerSocket source: jrt:/java.base
[0.041s][info   ][class,load] jdk.internal.access.JavaNetSocketAccess source: jrt:/java.base
[0.041s][info   ][class,load] java.net.ServerSocket$1 source: jrt:/java.base
[0.041s][info   ][class,load] java.net.SocketOptions source: jrt:/java.base
[0.041s][info   ][class,load] java.net.SocketImpl source: jrt:/java.base
[0.045s][info   ][class,load] java.net.SocketImpl$$Lambda$1/0x0000000800ba0840 source: java.net.SocketImpl
[0.048s][info   ][class,load] sun.net.PlatformSocketImpl source: jrt:/java.base
[0.048s][info   ][class,load] java.net.AbstractPlainSocketImpl source: jrt:/java.base

[0.048s][info   ][class,load] java.net.PlainSocketImpl source: jrt:/java.base

[0.048s][info   ][class,load] java.net.AbstractPlainSocketImpl$1 source: jrt:/java.base
[0.050s][info   ][class,load] sun.net.ext.ExtendedSocketOptions source: jrt:/java.base
[0.050s][info   ][class,load] jdk.net.ExtendedSocketOptions source: jrt:/jdk.net
[0.050s][info   ][class,load] java.net.SocketOption source: jrt:/java.base
[0.051s][info   ][class,load] jdk.net.ExtendedSocketOptions$ExtSocketOption source: jrt:/jdk.net
[0.051s][info   ][class,load] jdk.net.SocketFlow source: jrt:/jdk.net
[0.051s][info   ][class,load] jdk.net.ExtendedSocketOptions$PlatformSocketOptions source: jrt:/jdk.net
[0.051s][info   ][class,load] jdk.net.ExtendedSocketOptions$PlatformSocketOptions$1 source: jrt:/jdk.net
[0.051s][info   ][class,load] jdk.net.ExtendedSocketOptions$1 source: jrt:/jdk.net
[0.051s][info   ][class,load] java.net.StandardSocketOptions source: jrt:/java.base
[0.051s][info   ][class,load] java.net.StandardSocketOptions$StdSocketOption source: jrt:/java.base
[0.053s][info   ][class,load] sun.net.ext.ExtendedSocketOptions$$Lambda$2/0x0000000800ba1040 source: sun.net.ext.ExtendedSocketOptions
[0.056s][info   ][class,load] java.net.SocketAddress source: jrt:/java.base
[0.056s][info   ][class,load] java.net.InetSocketAddress source: jrt:/java.base
[0.058s][info   ][class,load] java.net.InetSocketAddress$InetSocketAddressHolder source: jrt:/java.base
[0.059s][info   ][class,load] java.net.SocketCleanable source: jrt:/java.base 
```

*P.S Java 15 [JEP 373:重新实现遗留的 DatagramSocket API](http://web.archive.org/web/20221124003658/https://openjdk.java.net/jeps/373) ，替换`java.net.DatagramSocket`和`java.net.MulticastSocket`的底层实现。*

*延伸阅读*

*   [JEP-353 重新实现传统套接字 API](http://web.archive.org/web/20221124003658/https://openjdk.java.net/jeps/353)

## 4。JEP-354 开关表情(预览)

Java 12 引入了 [JEP 325 开关表达式](http://web.archive.org/web/20221124003658/https://openjdk.java.net/jeps/325)。这个 JEP 放弃了`break value`而选择了`yield`关键字，从`switch`表达式中返回一个值。

这是 Java 13 中的一个预览语言特性

传统的`switch`语句，我们可以这样返回值:

```java
 private static String getNumber(int number) {
        String result = "";
        switch (number) {
            case 1:
            case 2:
                result = "one or two";
                break;
            case 3:
                result = "three";
                break;
            case 4:
            case 5:
            case 6:
                result = "four or five or six";
                break;
            default:
                result = "unknown";
        }
        ;
        return result;
    } 
```

在 Java 12 中，我们可以使用`break`从`switch`返回一个值。

```java
 private static String getNumberViaBreak(int number) {
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

在 Java 13 中，上面的 Java 12 `value break`被删除，取而代之的是`yield`关键字来返回值。

```java
 private static String getNumberViaYield(int number) {
      return switch (number) {
          case 1, 2:
              yield "one or two";
          case 3:
              yield "three";
          case 4, 5, 6:
              yield "four or five or six";
          default:
              yield "unknown";
      };
  } 
```

或者像这样

```java
 private static String getNumberViaYield2(int number) {
      return switch (number) {
          case 1, 2:
              yield "one or two";
          case 3:
              yield "three";
          case 4, 5, 6:
              int i = 0;
              i++;
              yield "four or five or six : " + i;
          default:
              yield "unknown";
      };
    } 
```

Java 13 中仍然支持规则标签或箭头或`case L`。

```java
 private static String getNumberViaCaseL(int number) {
      return switch (number) {
          case 1, 2 -> "one or two";
          case 3 -> "three";
          case 4, 5, 6 -> "four or five or six";
          default -> "unknown";
      };
  } 
```

或者像这样，混合使用箭头语法和`yield`。

```java
 private static String getNumberViaCaseL2(int number) {
      return switch (number) {
          case 1, 2 -> "one or two";
          case 3 -> "three";
          case 4, 5, 6 -> {
              int i = 0;
              i++;
              yield "four or five or six :" + 1;
          }
          default -> "unknown";
      };
  } 
```

这个开关表达式成为了 Java 14 [JEP 361](http://web.archive.org/web/20221124003658/https://openjdk.java.net/jeps/361) 的标准特性。

*延伸阅读*

*   [JEP-354 开关表情(预览)](http://web.archive.org/web/20221124003658/https://openjdk.java.net/jeps/354)
*   [Java 13 开关表达式](http://web.archive.org/web/20221124003658/https://www.mkyong.com/java/java-13-switch-expressions/)

## 5。JEP-355 文本块(预览)

这个 JEP 最后引入了一个多行字符串文字，一个文本块。

这个文本块在 Java 15 中是一个永久的特性。

Java 13 之前

```java
 String html ="<html>\n" +
			  "   <body>\n" +
			  "      <p>Hello, World</p>\n" +
			  "   </body>\n" +
			  "</html>\n";

 String json ="{\n" +
			  "   \"name\":\"mkyong\",\n" +
			  "   \"age\":38\n" +
			  "}\n"; 
```

现在是 Java 13

```java
 String html =  """
                <html>
                    <body>
                        <p>Hello, World</p>
                    </body>
                </html>
				        """;

 String json = """
                {
                    "name":"mkyong",
                    "age":38
                }
                """; 
```

**注意**
这个文本块在[Java 14–JEP 368](http://web.archive.org/web/20221124003658/https://openjdk.java.net/jeps/368)中有了第二个预览，增加了两个新的转义序列:

*   `\<end-of-line>`取消线路终止。
*   `\s`翻译成单个空格。

要启用 Java 13 预览功能:

```java
 javac --enable-preview --release 13 Example.java
java --enable-preview Example 
```

*延伸阅读*

*   [JEP-355 文本块(预览)](http://web.archive.org/web/20221124003658/https://openjdk.java.net/jeps/355)
*   [多行字符串、文本块示例](/web/20221124003658/https://mkyong.com/java/java-multi-line-string-text-blocks/)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221124003658/https://github.com/mkyong/core-java)

$ cd java-13

## 参考文献

*   [OpenJDK 13 项目](http://web.archive.org/web/20221124003658/https://openjdk.java.net/projects/jdk/13/)
*   Oracle–Java 13 的到来！
*   [cl4cds](http://web.archive.org/web/20221124003658/https://simonis.github.io/cl4cds/)
*   [Java 版本历史](http://web.archive.org/web/20221124003658/https://en.wikipedia.org/wiki/Java_version_history)
*   [Java 13 开关表达式](http://web.archive.org/web/20221124003658/https://www.mkyong.com/java/java-13-switch-expressions/)
*   [Java 13 与应用程序类-数据共享](http://web.archive.org/web/20221124003658/https://blog.codefx.org/java/application-class-data-sharing/)
*   [Java 13 文本块](http://web.archive.org/web/20221124003658/https://blog.codefx.org/java/text-blocks/)
*   Java 安全性有什么新特性？
*   [Java 多行字符串、文本块示例](/web/20221124003658/https://mkyong.com/java/java-multi-line-string-text-blocks/)

<input type="hidden" id="mkyong-current-postId" value="15190">