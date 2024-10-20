# Java 14 的新特性是什么

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/what-is-new-in-java-14/>

![Java 14 logo](img/a0c2c22c5dd44cd53a1e348dc99ae33b.png)

[Java 14](http://web.archive.org/web/20221124003654/https://openjdk.java.net/projects/jdk/14/) 于 2020 年 3 月 17 日正式上市，[点击此处](http://web.archive.org/web/20221124003654/https://jdk.java.net/14/)下载 Java 14。

Java 14 特性。

*   [1。JEP 305:实例的模式匹配(预览)](#jep-305-pattern-matching-for-instanceof-preview)
*   [2。JEP 343:包装工具(保温箱)](#jep-343-packaging-tool-incubator)
*   [3。JEP 345:G1 的 NUMA 感知内存分配](#jep-345numa-aware-memory-allocation-for-g1)
*   [4。JEP 349: JFR 事件流](#jep-349-jfr-event-streaming)
*   [5。JEP 352:非易失映射字节缓冲区](#jep-352non-volatile-mapped-byte-buffers)
*   [6。JEP 358:有用的空指针异常](#jep-358-helpful-nullpointerexceptions)
*   [7。JEP 359:记录(预览)](#jep-359records-preview)
*   [8。JEP 361:开关表达式(标准)](#jep-361-switch-expressions-standard)
*   [9。JEP 362:反对 Solaris 和 SPARC 端口](#jep-362-deprecate-the-solaris-and-sparc-ports)
*   10。JEP 363:移除并发标记清除(CMS)垃圾收集器
*   [11。JEP 364:苹果电脑上的 ZGC(实验版)](#jep-364-zgc-on-macos-experimental)
*   [12。JEP 365:Windows 上的 ZGC(实验)](#jep-365-zgc-on-windows-experimental)
*   13。JEP 366:反对 ParallelScavenge + SerialOld GC 组合
*   [14。JEP 367:移除 Pack200 工具和 API](#jep-367-remove-the-pack200-tools-and-api)
*   15。JEP 368:文本块(第二次预览)
*   16。JEP 370:外部内存访问 API(孵化器)

*Java 14 开发者特性。*

切换表达式(标准)、记录(预览)、模式匹配(预览)、文本块或多行(第二次预览)、外部内存访问 API(孵化器)

## 1。JEP 305:实例的模式匹配(预览)

在 Java 14 之前，我们使用`instanceof-and-cast`来检查对象的类型并转换为变量。

```java
 if (obj instanceof String) {        // instanceof
    String s = (String) obj;          // cast
    if("jdk14".equalsIgnoreCase(s)){
        //...
    }
  }else {
	   System.out.println("not a string");
  } 
```

现在 Java 14，我们可以像这样重构上面的代码:

```java
 if (obj instanceof String s) {      // instanceof, cast and bind variable in one line.
      if("jdk4".equalsIgnoreCase(s)){
          //...
      }
  }else {
	   System.out.println("not a string");
  } 
```

如果`obj`是`String`的一个实例，那么它被转换为`String`，并被赋给绑定变量`s`。

*历史*
这种模式匹配是 Java 16 中的标准特性， [JEP 394](http://web.archive.org/web/20221124003654/https://mkyong.com/java/what-is-new-in-java-16/#jep-394-pattern-matching-for-instanceof) 。

*延伸阅读*

*   [JEP 305:实例的模式匹配(预览)](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/305)

## 2。JEP 343:包装工具(保温箱)

新的`jpackage`工具，用于将 Java 应用打包成特定于平台的包，例如:

*   Linux: deb 和 rpm
*   macOS: pkg 和 dmg
*   Windows: msi 和 exe

*P.S 这个打包工具是 Java 16 的标准特性，[访问这个`jpackage`例子](http://web.archive.org/web/20221124003654/https://mkyong.com/java/what-is-new-in-java-16/#jep-392-packaging-tool)。*

*延伸阅读*

*   [JEP 343:包装工具(保温箱)](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/343)

## 3。JEP 345:G1 的 NUMA 感知内存分配

新的 [NUMA 感知的内存](http://web.archive.org/web/20221124003654/https://en.wikipedia.org/wiki/Non-uniform_memory_access)分配模式，提高了大型机器上的 G1 性能。添加`+XX:+UseNUMA`选项将其启用。

*延伸阅读*

*   [JEP 345:G1 的 NUMA 感知内存分配](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/345)

## 4。JEP 349: JFR 事件流

改进了现有的 JFR 以支持事件流，这意味着现在我们可以实时传输 JFR 事件，而无需将记录的事件转储到磁盘并手动解析。

JDK 飞行记录器(JFR)是一个工具，用于收集关于正在运行的 Java 应用程序的诊断和分析数据。通常，我们开始记录，停止记录，将记录的事件转储到磁盘进行解析，这对于剖析、分析或调试非常有用。

*延伸阅读*

*   [JEP 349: JFR 事件流](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/349)
*   JEP 328:飞行记录器

## 5。JEP 352:非易失映射字节缓冲区

改进的`FileChannel` API 创建了访问[非易失性存储器(NVM)](http://web.archive.org/web/20221124003654/https://en.wikipedia.org/wiki/Non-volatile_memory) 的`MappedByteBuffer`，这是一种即使经过电源循环也能检索存储数据的存储器。例如，该特性确保了可能仍在缓存中的任何更改都被写回到内存中。

*P.S 只有 Linux/x64 和 Linux/AArch64 OS 支持这个！*

*延伸阅读*

*   [JEP 352:非易失性映射字节缓冲器](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/352)

## 6。JEP 358:有用的空指针异常

通过告知哪个变量为空，改进了对`NullPointerExceptions`的描述。添加`-XX:+ShowCodeDetailsInExceptionMessages`选项以启用该功能。

抛出`NullPointerException`的简单 Java 文件。

Test.java

```java
 import java.util.Locale;

public class Test {

    public static void main(String[] args) {

        String input = null;
        String result = showUpperCase(input); // NullPointerException
        System.out.println(result);

    }

    public static String showUpperCase(String str){
        return str.toUpperCase(Locale.US);
    }

} 
```

Java 14 之前。

```java
 $ /usr/lib/jvm/jdk-14/bin/java Test

Exception in thread "main" java.lang.NullPointerException
	at Test.showUpperCase(Test.java:15)
	at Test.main(Test.java:9) 
```

带`-XX:+ShowCodeDetailsInExceptionMessages`选项的 Java 14。

```java
 $ /usr/lib/jvm/jdk-14/bin/java -XX:+ShowCodeDetailsInExceptionMessages Test

Exception in thread "main" java.lang.NullPointerException:
  Cannot invoke "String.toUpperCase(java.util.Locale)" because "<parameter1>" is null
	at Test.showUpperCase(Test.java:15)
	at Test.main(Test.java:9) 
```

*延伸阅读*

*   [JEP 358:有用的空指针异常](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/358)

## 7。JEP 359:记录(预览)

一个名为`record`(又名数据类)的新类，它是一个最终类，不是抽象的，它的所有字段也是最终的。`record`会在编译时自动生成繁琐的`constructors`、`public get`、`equals()`、`hashCode()`、`toString()`。

没有设置者，所有字段都是最终的。

一个`record`或数据类，创建类名和变量，我们就可以开始使用它了。

Point.java

```java
 record Point(int x, int y) { } 
```

Test.java

```java
 public class Test {

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

用`--enable-preview`测试一下。

```java
 $ /usr/lib/jvm/jdk-14/bin/javac --enable-preview --release 14 Point.java
$ /usr/lib/jvm/jdk-14/bin/javac --enable-preview --release 14 Test.java

$ /usr/lib/jvm/jdk-14/bin/java --enable-preview Test 
```

输出

```java
 10
20
11
22
10
20
330
363
330
false
true
false 
```

*历史*

*   Java 15 [JEP 384](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/384) 对记录进行了第二次预览，增加了新的特性，比如密封类型、本地记录、记录注释和反射 API。
*   Java 16 [JEP 395](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/395) ，记录或数据类是标准特性。

*延伸阅读*

*   [JEP 359:记录(预览)](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/359)
*   [Java 14 记录数据类](/web/20221124003654/https://mkyong.com/java/java-14-record-data-class/)

## 8。JEP 361:开关表达式(标准)

switch 表达式是 Java 12 和 Java 13 中的预览特性；现在它正式成为了 JDK 的标准特性，这意味着从 Java 14 开始，我们可以通过 switch 表达式返回值，而不需要指定`--enable-preview`选项。

查看摘要；我们可以使用`yield`从`switch`返回一个值。

```java
 private static int getValueViaYield(String mode) {
        int result = switch (mode) {
            case "a", "b":
                yield 1;
            case "c":
                yield 2;
            case "d", "e", "f":
								// do something here...
							 	System.out.println("Supports multi line block!");
                yield 3;
            default:
                yield -1;
        };
        return result;
    } 
```

或者通过标签规则或箭头。

```java
 private static int getValueViaArrow(String mode) {
        int result = switch (mode) {
            case "a", "b" -> 1;
            case "c" -> 2;
            case "d", "e", "f" -> {
                // do something here...
                System.out.println("Supports multi line block!");
                yield 3;
            }
            default -> -1;
        };
        return result;
    } 
```

*延伸阅读*

*   [JEP 361:开关表达式(标准)](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/361)
*   [Java 13 开关表达式](/web/20221124003654/https://mkyong.com/java/java-13-switch-expressions/)

## 9。JEP 362:反对 Solaris 和 SPARC 端口

放弃对 Solaris/SPARC、Solaris/x64 和 Linux/SPARC 端口的支持，更少的平台支持意味着新功能的交付速度更快。

*P.S Java 15 [JEP 381](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/381) 移除上述端口。*

*延伸阅读*

*   JEP 362:反对索拉里斯和 SPARC 港口

## 10。JEP 363:移除并发标记清除(CMS)垃圾收集器

Java 9 [JEP 291](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/291) 弃用了这个并发标记清除(CMS)垃圾收集器，现在它被正式移除了。

```java
 /usr/lib/jvm/jdk-14/bin/java -XX:+UseConcMarkSweepGC Test

OpenJDK 64-Bit Server VM warning: Ignoring option UseConcMarkSweepGC; support was removed in 14.0 
```

*延伸阅读*

*   [JEP 363:移除并发标记清除(CMS)垃圾收集器](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/363)

## 11。JEP 364:苹果电脑上的 ZGC(实验版)

Java 11 [JEP 333](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/333) 在 Linux 上引入了 Z 垃圾收集器(ZGC ),现在它可以移植到 macOS。

这个 ZGC 是 Java 15 [JEP 377](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/377) 的一个产品特性。

*延伸阅读*

*   [JEP 364:苹果电脑上的 ZGC(实验版)](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/364)

## 12。JEP 365:Windows 上的 ZGC(实验)

Java 11 [JEP 333](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/333) 在 Linux 上引入了 Z 垃圾收集器(ZGC)，现在移植到 Windows 版本> = 1803。

这个 ZGC 是 Java 15 [JEP 377](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/377) 的一个产品特性。

*延伸阅读*

*   [JEP 365:Windows 上的 ZGC(实验)](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/365)

## 13。JEP 366:反对 ParallelScavenge + SerialOld GC 组合

由于较少使用和大量维护工作，Java 14 不赞成并行年轻一代和串行老一代 GC 算法的组合。

```java
 /usr/lib/jvm/jdk-14/bin/java -XX:-UseParallelOldGC Test

OpenJDK 64-Bit Server VM warning: Option UseParallelOldGC was deprecated in version 14.0 and will likely be removed in a future release. 
```

*延伸阅读*

*   [JEP 366:反对 ParallelScavenge + SerialOld GC 组合](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/366)

## 14。JEP 367:移除 Pack200 工具和 API

Java 11 [JEP 336](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/336) 弃用了`pack200`和`unpack200`工具，以及`java.util.jar`包中的`Pack200` API，现在正式移除。

*延伸阅读*

*   JEP 367:移除 Pack200 工具和 API

## 15。JEP 368:文本块(第二次预览)

第一个预览版出现在 Java 13 [JEP 354](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/354) 中，现在增加了两个新的转义序列:

*   `\<end-of-line>`取消线路终止。
*   `\s`翻译成单个空格。

Test.java

```java
 public class Test {

    public static void main(String[] args) {

        String html = "<html>\n" +
                      "   <body>\n" +
                      "      <p>Hello, World</p>\n" +
                      "   </body>\n" +
                      "</html>\n";

        String java13 = """
                        <html>
                            <body>
                                <p>Hello, World</p>
                            </body>
                        </html>
                        """;

        String java14 = """
                        <html>
                            <body>\
                                <p>Hello, '\s' World</p>\
                            </body>
                        </html>
                        """;

        System.out.println(html);
        System.out.println(java13);
        System.out.println(java14);

    }

} 
```

```java
 $ /usr/lib/jvm/jdk-14/bin/javac --enable-preview --release 14 Test.java
$ /usr/lib/jvm/jdk-14/bin/java --enable-preview Test 
```

输出

```java
 html>
   <body>
      <p>Hello, World</p>
   </body>
</html>

<html>
    <body>
        <p>Hello, World</p>
    </body>
</html>

<html>
    <body>        <p>Hello, ' ' World</p>    </body>
</html> 
```

这个文本块在 Java 15 中是一个永久的特性。

*延伸阅读*

*   [JEP 368:文本块(第二次预览)](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/368)
*   [Java 多行字符串，文本块](/web/20221124003654/https://mkyong.com/java/java-multi-line-string-text-blocks/)

## 16。JEP 370:外部内存访问 API(孵化器)

一个[孵化器模块](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/11)，允许 Java API 访问 Java 堆之外的外来内存。

Test.java

```java
 import jdk.incubator.foreign.*;
import java.lang.invoke.VarHandle;
import java.nio.ByteOrder;

public class Test {

    public static void main(String[] args) {

	     VarHandle intHandle = MemoryHandles.varHandle(
            int.class, ByteOrder.nativeOrder());

        try (MemorySegment segment = MemorySegment.allocateNative(1024)) {

            MemoryAddress base = segment.baseAddress();

            // print memory address
            System.out.println(base);                 

            // set value 999 into the foreign memory
	          intHandle.set(base, 999);                 

            // get the value from foreign memory
            System.out.println(intHandle.get(base));  

        }

    }

} 
```

用孵化器模块`jdk.incubator.foreign`编译运行。

```java
 $ /usr/lib/jvm/jdk-14/bin/javac --add-modules jdk.incubator.foreign Test.java

warning: using incubating module(s): jdk.incubator.foreign
1 warning

$ /usr/lib/jvm/jdk-14/bin/java --add-modules jdk.incubator.foreign Test

WARNING: Using incubator modules: jdk.incubator.foreign
MemoryAddress{ region: MemorySegment{ id=0x4aac6dca limit: 1024 } offset=0x0 }
999 
```

*历史*

*   Java 15 [JEP 383](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/383) 拥有第二个孵化器。
*   Java 16 [JEP 393](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/393) 拥有第三个孵化器。

*延伸阅读*

*   [JEP 370:外来内存访问 API(孵化器)](http://web.archive.org/web/20221124003654/https://openjdk.java.net/jeps/370)
*   [jdk .孵化器. foreign Javadoc](http://web.archive.org/web/20221124003654/https://download.java.net/java/GA/jdk14/docs/api/jdk.incubator.foreign/jdk/incubator/foreign/package-summary.html)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221124003654/https://github.com/mkyong/core-java)

$ cd java-14

## 参考文献

*   [OpenJDK 14 项目](http://web.archive.org/web/20221124003654/https://openjdk.java.net/projects/jdk/14/)
*   Java 14 带来了一系列新特性
*   [Java 版本历史](http://web.archive.org/web/20221124003654/https://en.wikipedia.org/wiki/Java_version_history)
*   [维基百科–非易失性存储器(NVM)](http://web.archive.org/web/20221124003654/https://en.wikipedia.org/wiki/Non-volatile_memory)
*   [维基百科–非统一内存访问(NUMA)](http://web.archive.org/web/20221124003654/https://en.wikipedia.org/wiki/Non-uniform_memory_access)
*   【Java 的数据类和密封类型
*   [jdk .孵化器.外国](http://web.archive.org/web/20221124003654/https://download.java.net/java/GA/jdk14/docs/api/jdk.incubator.foreign/jdk/incubator/foreign/package-summary.html)
*   [Java 13 开关表达式](http://web.archive.org/web/20221124003654/https://www.mkyong.com/java/java-13-switch-expressions/)
*   [Java 13 文本块](http://web.archive.org/web/20221124003654/https://blog.codefx.org/java/text-blocks/)

<input type="hidden" id="mkyong-current-postId" value="15592">