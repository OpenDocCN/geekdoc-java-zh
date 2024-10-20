# Java 17 的新特性是什么

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/what-is-new-in-java-17/>

![Java 17 logo](img/b7c5f15a0b521dfc9999c9cbddd44eb7.png)

[Java 17](http://web.archive.org/web/20221230032425/https://openjdk.java.net/projects/jdk/17/) 是一个长期支持(LTS)版本，于 2021 年 9 月 14 日全面上市，[点击此处](http://web.archive.org/web/20221230032425/https://jdk.java.net/17/)下载 Java 17。

Java 17 有 14 个 JEP 项目。

*   [1。JEP 306:恢复总是严格的浮点语义](#jep-306-restore-always-strict-floating-point-semantics)
*   [2。JEP 356:增强型伪随机数发生器](#jep-356-enhanced-pseudo-random-number-generators)
*   [3。JEP 382:新的 macOS 渲染管道](#jep-382-new-macos-rendering-pipeline)
*   [4。JEP 391: macOS/AArch64 端口](#jep-391-macosaarch64-port)
*   [5。JEP 398:反对删除小程序 API](#jep-398-deprecate-the-applet-api-for-removal)
*   [6。JEP 403:强力封装 JDK 内部构件](#jep-403-strongly-encapsulate-jdk-internals)
*   7 .[。JEP 406:开关模式匹配(预览)](#jep-406-pattern-matching-for-switch-preview)
    *   [7.1 如果…否则链](#ifelse-chain)
    *   [7.2 模式匹配和空值](#pattern-matching-and-null)
    *   [7.3 开关中的细化模式](#refining-patterns-in-switch)
*   [8。JEP 407:移除 RMI 激活](#jep-407-remove-rmi-activation)
*   [9。JEP 409:密封类](#jep-409-sealed-classes)
*   10。JEP 410:移除实验性的 AOT 和 JIT 编译器
*   [11。JEP 411:反对移除安全管理器](#jep-411-deprecate-the-security-manager-for-removal)
*   [12。JEP 412:外来函数&内存 API(孵化器)](#jep-412-foreign-function-memory-api-incubator)
*   13。JEP 414: Vector API(第二个孵化器)
*   [14。JEP 415:特定于上下文的反序列化过滤器](#jep-415-context-specific-deserialization-filters)
*   [下载源代码](#download-source-code)
*   [参考文献](#references)

*Java 17 开发者特性。*
伪随机数发生器，用于切换的模式匹配(预览)，密封类(标准特性)，外来函数&内存 API(孵化器)，动态反序列化过滤器。

## 1。JEP 306:恢复总是严格的浮点语义

这个 JEP 用于数字敏感的程序，主要用于科学目的；它再次使默认浮点运算变得严格或`Strictfp`，确保每个平台上的浮点计算结果相同。

*简史*

1.  在 Java 1.2 之前，所有的浮点计算都是严格的；它还会导致基于 x87 的硬件过热。
2.  从 Java 1.2 开始，我们需要关键字`strictfp`来启用严格的浮点计算。默认的浮点计算从严格的浮点计算更改为略微不同的浮点计算(避免过热问题)。
3.  现在，由于 Intel 和 AMD 都支持 [SSE2(流 SIMD 扩展 2)](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/SSE2) 扩展，这可以支持严格的 JVM 浮点操作而没有过热，所以，以前(Java 1.2 之前)基于 x87 的硬件上的过热问题在现在的硬件中是不敬的。
4.  Java 17 将 Java 1.2 之前的严格浮点计算恢复为默认，这意味着关键字`strictfp`现在是可选的。

**延伸阅读**

*   JEP 306:恢复总是严格的浮点语义
*   [维基百科–strictfp](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Strictfp)

## 2。JEP 356:增强型伪随机数发生器

这个 JEP 引入了一个名为`RandomGenerator`的新接口，使未来的[伪随机数发生器(PRNG)](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Pseudorandom_number_generator) 算法更容易实现或使用。

RandomGenerator.java

```java
 package java.util.random;

public interface RandomGenerator {
  //...
} 
```

下面的例子使用新的 Java 17 `RandomGeneratorFactory`来获得著名的`Xoshiro256PlusPlus` PRNG 算法，以生成特定范围(0–10)内的随机整数。

JEP356.java

```java
 package com.mkyong.java17.jep356;

import java.util.random.RandomGenerator;
import java.util.random.RandomGeneratorFactory;

public class JEP356 {

  public static void main(String[] args) {

      // legacy
      // RandomGeneratorFactory.of("Random").create(42);

      // default L32X64MixRandom
      // RandomGenerator randomGenerator = RandomGeneratorFactory.getDefault().create();

      // Passing the same seed to random, and then calling it will give you the same set of numbers
      // for example, seed = 999
      RandomGenerator randomGenerator = RandomGeneratorFactory.of("Xoshiro256PlusPlus").create(999);

      System.out.println(randomGenerator.getClass());

      int counter = 0;
      while(counter<=10){
          // 0-10
          int result = randomGenerator.nextInt(11);
          System.out.println(result);
          counter++;
      }

  }
} 
```

输出

Terminal

```java
 class jdk.random.Xoshiro256PlusPlus
4
6
9
5
7
6
5
0
6
10
4 
```

下面的代码生成了所有的 Java 17 PRNG 算法。

```java
 RandomGeneratorFactory.all()
              .map(fac -> fac.group()+ " : " +fac.name())
              .sorted()
              .forEach(System.out::println); 
```

输出

Terminal

```java
 LXM : L128X1024MixRandom
LXM : L128X128MixRandom
LXM : L128X256MixRandom
LXM : L32X64MixRandom
LXM : L64X1024MixRandom
LXM : L64X128MixRandom
LXM : L64X128StarStarRandom
LXM : L64X256MixRandom
Legacy : Random
Legacy : SecureRandom
Legacy : SplittableRandom
Xoroshiro : Xoroshiro128PlusPlus
Xoshiro : Xoshiro256PlusPlus 
```

Java 17 还重构了传统的随机类，如`java.util.Random`、`SplittableRandom`和`SecureRandom`，以扩展新的`RandomGenerator`接口。

**延伸阅读**

*   [JEP 356:增强型伪随机数发生器](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/356)
*   [Java 17 的增强伪随机数生成器](http://web.archive.org/web/20221230032425/https://mbien.dev/blog/entry/enhanced-pseudo-random-number-generators)

## 3。JEP 382:新的 macOS 渲染管道

苹果在 [macOS 10.14 版本](http://web.archive.org/web/20221230032425/https://developer.apple.com/documentation/macos-release-notes/macos-mojave-10_14-release-notes#Open-GL-and-Open-CL)(2018 年 9 月)中弃用了 OpenGL 渲染库，转而支持新的[金属框架](http://web.archive.org/web/20221230032425/https://developer.apple.com/metal/)以获得更好的性能。

这款 JEP 将 macOS 的 Java 2D(类似 Swing GUI)内部渲染管道从苹果 OpenGL API 改为苹果 Metal API 这是一种内在的变化；没有新的 Java 2D API，也没有改变任何现有的 API。

**延伸阅读**

*   [JEP 382:新的 macOS 渲染管道](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/382)

## 4。JEP 391: macOS/AArch64 端口

苹果有一个长期的计划，将它的 Mac 从 x64 过渡到 AArch64(例如，苹果 M1 处理器)。

这个 JEP 港 JDK 运行在 macOS 上的 AArch64 平台。

**延伸阅读**

*   [JEP 391: macOS/AArch64 端口](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/391)

## 5。JEP 398:反对删除小程序 API

[Java Applet API](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Java_applet) 是不相关的，因为大多数网络浏览器已经移除了对 Java 浏览器插件的支持。

Java 9 弃用了 Applet API。

```java
 @Deprecated(since = "9")
public class Applet extends Panel {
  //...
} 
```

此 JEP 将 Applet API 标记为删除。

```java
 @Deprecated(since = "9", forRemoval = true)
@SuppressWarnings("removal")
public class Applet extends Panel {
  //...
} 
```

**延伸阅读**

*   [JEP 398:反对删除小应用程序 API](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/398)

## 6。JEP 403:强力封装 JDK 内部构件

许多第三方库、框架和工具正在访问 JDK 的内部 API 和包。Java 16、 [JEP 396](http://web.archive.org/web/20221230032425/https://mkyong.com/java/what-is-new-in-java-16/#jep-396-strongly-encapsulate-jdk-internals-by-default) 默认做强封装(我们不允许轻易访问内部 API)。然而，我们仍然可以使用`--illegal-access`切换到简单封装，仍然可以访问内部 API。

这个 JEP 是上述 Java 16 JEP 396 的继任者，它通过移除`--illegal-access`选项多了一步，这意味着我们没有办法访问内部 API，除了像`sun.misc.Unsafe`这样的关键内部 API。

试试 Java 17 中的`--illegal-access=warn`。

Terminal

```java
 java --illegal-access=warn

OpenJDK 64-Bit Server VM warning: Ignoring option --illegal-access=warn; support was removed in 17.0 
```

**延伸阅读**

*   [JEP 403:强力封装 JDK 内部构件](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/403)

## 7。JEP 406:开关模式匹配(预览)

这个 JEP 为`switch`语句和表达式添加了模式匹配。由于这是一个预览功能，我们需要使用`--enable-preview`选项来启用它。

### 7.1 如果…否则链

在 Java 17 之前，我们通常会针对几种可能性使用一系列的`if...else`测试。

JEP406.java

```java
 package com.mkyong.java17.jep406;

public class JEP406 {

  public static void main(String[] args) {

      System.out.println(formatter("Java 17"));
      System.out.println(formatter(17));

  }

  static String formatter(Object o) {
      String formatted = "unknown";
      if (o instanceof Integer i) {
          formatted = String.format("int %d", i);
      } else if (o instanceof Long l) {
          formatted = String.format("long %d", l);
      } else if (o instanceof Double d) {
          formatted = String.format("double %f", d);
      } else if (o instanceof String s) {
          formatted = String.format("String %s", s);
      }
      return formatted;
  }

} 
```

在 Java 17 中，我们可以这样重写上面的代码:

JEP406.java

```java
 package com.mkyong.java17.jep406;

public class JEP406 {

    public static void main(String[] args) {

        System.out.println(formatterJava17("Java 17"));
        System.out.println(formatterJava17(17));

    }

    static String formatterJava17(Object o) {
        return switch (o) {
            case Integer i -> String.format("int %d", i);
            case Long l    -> String.format("long %d", l);
            case Double d  -> String.format("double %f", d);
            case String s  -> String.format("String %s", s);
            default        -> o.toString();
        };
    }

} 
```

### 7.2 模式匹配和空值

现在我们可以直接测试`switch`中的`null`。

旧抄本

JEP406.java

```java
 package com.mkyong.java17.jep406;

public class JEP406 {

  public static void main(String[] args) {

      testString("Java 16");  // Ok
      testString("Java 11");  // LTS
      testString("");         // Ok
      testString(null);       // Unknown!
  }

  static void testString(String s) {
      if (s == null) {
          System.out.println("Unknown!");
          return;
      }
      switch (s) {
          case "Java 11", "Java 17"   -> System.out.println("LTS");
          default                     -> System.out.println("Ok");
      }
  }

} 
```

新代码。

JEP406.java

```java
 package com.mkyong.java17.jep406;

public class JEP406 {

    public static void main(String[] args) {

        testStringJava17("Java 16");  // Ok
        testStringJava17("Java 11");  // LTS
        testStringJava17("");         // Ok
        testStringJava17(null);       // Unknown!
    }

    static void testStringJava17(String s) {
        switch (s) {
            case null                   -> System.out.println("Unknown!");
            case "Java 11", "Java 17"   -> System.out.println("LTS");
            default                     -> System.out.println("Ok");
        }
    }

} 
```

### 7.3 开关中的细化模式

查看下面的代码片段。为了测试`Triangle t`和`t.calculateArea()`，我们需要创建一个额外的`if`条件。

```java
 class Shape {}
  class Rectangle extends Shape {}
  class Triangle  extends Shape {
      int calculateArea(){
          //...
      } }

  static void testTriangle(Shape s) {
      switch (s) {
          case null:
              break;
          case Triangle t:
              if (t.calculateArea() > 100) {
                  System.out.println("Large triangle");
                  break;
              }else{
                  System.out.println("Triangle");
              }
          default:
              System.out.println("Unknown!");
      }
  } 
```

Java 17 允许所谓的重新定义模式或`guarded patterns`如下:

```java
 static void testTriangle2(Shape s) {
      switch (s) {
          case null ->
                  {}
          case Triangle t && (t.calculateArea() > 100) ->
                  System.out.println("Large triangle");
          case Triangle t ->
                  System.out.println("Triangle");
          default ->
                  System.out.println("Unknown!");
      }
  } 
```

**延伸阅读**

欲了解更多示例和解释，请访问此 [JEP 406:开关模式匹配(预览)](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/406)

## 8。JEP 407:移除 RMI 激活

Java 15， [JEP385](http://web.archive.org/web/20221230032425/https://mkyong.com/java/what-is-new-in-java-15/#jep-385deprecate-rmi-activation-for-removal) 不赞成删除 [RMI 激活](http://web.archive.org/web/20221230032425/https://docs.oracle.com/en/java/javase/14/docs/specs/rmi/activation.html)。

这个 JEP 移除了 RMI 激活或`java.rmi.activation`包。

**延伸阅读**

*   [JEP 407:移除 RMI 激活](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/407)

## 9。JEP 409:密封类

Java 15、 [JEP 360](#https://mkyong.com/java/what-is-new-in-java-15/#jep-360-sealed-classes-preview) 和 Java 16、 [JEP 397](#https://mkyong.com/java/what-is-new-in-java-16/#jep-397-sealed-classes-second-preview) 引入了【密封类(https://Cr . open JDK . Java . net/~ briangoetz/amber/datum . html)作为预览功能。

这个 JEP 最终将密封类作为 Java 17 的标准特性，与 Java 16 没有任何变化。

密封的类和接口控制或限制谁可以成为子类型。

```java
 public sealed interface Command
    permits LoginCommand, LogoutCommand, PluginCommand{
    //...
} 
```

**延伸阅读**

*   [JEP 409:密封类](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/409)

## 10。JEP 410:移除实验性的 AOT 和 JIT 编译器

Java 9， [JEP 295](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/295) 引入了超前编译(`jaotc`工具)作为一个实验特性。后来的 Java 10， [JEP 317](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/317) 再次提出它作为实验性的 JIT 编译器。

然而，这个特性自从被引入以来几乎没有什么用处，并且需要大量的工作来维护它，所以这个 JEP 移除了实验性的基于 Java 的[提前(AOT)](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Ahead-of-time_compilation) 和[实时(JIT)](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Just-in-time_compilation) 编译器

下列 AOT 软件包、类、工具和代码被删除:

*   `jdk.aot`—jaotc 工具
*   `jdk.internal.vm.compiler`—Graal 编译器
*   `jdk.internal.vm.compiler.management` —圣杯的 MBean
*   `src/hotspot/share/aot` —转储和加载 AOT 代码
*   由`#if INCLUDE_AOT`保护的附加代码

**延伸阅读**

*   [JEP 410:移除实验性的 AOT 和 JIT 编译器](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/410)

## 11。JEP 411:反对移除安全管理器

Java 1.0 引入了[安全管理器](http://web.archive.org/web/20221230032425/https://docs.oracle.com/javase/tutorial/essential/environment/security.html)来保护客户端 Java 代码，现在已经无关紧要了。

这个 JEP 反对安全管理器被删除。

SecurityManager.java

```java
 package java.lang;

 * @since   1.0
 * @deprecated The Security Manager is deprecated and subject to removal in a
 *       future release. There is no replacement for the Security Manager.
 *       See <a href="https://openjdk.java.net/jeps/411">JEP 411</a> for
 *       discussion and alternatives.
 */
@Deprecated(since="17", forRemoval=true)
public class SecurityManager {
  //...
} 
```

**延伸阅读**

*   [JEP 411:反对安全经理被免职](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/411)

## 12。JEP 412:外来函数&内存 API(孵化器)

这个外来函数和内存 API 允许开发人员访问 JVM 外部的代码(外来函数)、存储在 JVM 外部的数据(堆外数据)，以及访问不受 JVM 管理的内存(外来内存)。

这是一个孵化的特征；需要添加`--add-modules jdk.incubator.foreign`来编译和运行 Java 代码。

*历史*

*   Java 14 [JEP 370](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/370) 引入了外来内存访问 API(孵化器)。
*   Java 15 [JEP 383](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/383) 引入了外来内存访问 API(第二孵化器)。
*   Java 16 [JEP 389](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/389) 引入了国外的链接器 API(孵化器)。
*   Java 16 [JEP 393](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/393) 引入了外来内存访问 API(第三孵化器)。
*   Java 17 [JEP 412](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/412) 引入了外来函数&内存 API(孵化器)。

请参考之前 Java 16 中的[外来链接器 API 示例。](http://web.archive.org/web/20221230032425/https://mkyong.com/java/what-is-new-in-java-16/#jep-389-foreign-linker-api-incubator)

**延伸阅读**

*   [JEP 412:外来函数&内存 API(孵化器)](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/412)

## 13。JEP 414: Vector API(第二个孵化器)

Java 16， [JEP 414](#) 引入新的 Vector API 作为[孵化 API](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/11) 。

这个 JEP 改进了 Vector API 性能和其他增强功能，比如支持字符操作、将字节向量与布尔数组相互转换等。

**延伸阅读**

*   [JEP 414:载体 API(第二孵育器)](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/414)
*   [维基百科–自动矢量化](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Automatic_vectorization)
*   [Java 中的自动矢量化](http://web.archive.org/web/20221230032425/http://daniel-strecker.com/blog/2020-01-14_auto_vectorization_in_java/)

## 14。JEP 415:特定于上下文的反序列化过滤器

在 Java 中，反序列化不受信任的数据是危险的，阅读[OWASP-不受信任数据的反序列化](http://web.archive.org/web/20221230032425/https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data)和[Brian Goetz-走向更好的序列化](http://web.archive.org/web/20221230032425/https://cr.openjdk.java.net/~briangoetz/amber/serialization.html)。

Java 9， [JEP 290](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/290) 引入了序列化过滤来帮助防止反序列化漏洞。

14.1 以下示例使用模式创建了一个自定义过滤器。

DdosExample.java

```java
 package com.mkyong.java17.jep415;

import java.io.Serializable;

public class DdosExample implements Serializable {
  @Override
  public String toString() {
      return "running ddos...!";
  }
} 
```

JEP290.java

```java
 package com.mkyong.java17.jep415;

import java.io.*;

public class JEP290 {

  public static void main(String[] args) throws IOException {

      byte[] bytes = convertObjectToStream(new DdosExample());
      InputStream is = new ByteArrayInputStream(bytes);
      ObjectInputStream ois = new ObjectInputStream(is);

      // Setting a Custom Filter Using a Pattern
      // need full package path
      // the maximum number of bytes in the input stream = 1024
      // allows classes in com.mkyong.java17.jep415.*
      // allows classes in the java.base module
      // rejects all other classes !*
      ObjectInputFilter filter1 =
              ObjectInputFilter.Config.createFilter(
                  "maxbytes=1024;com.mkyong.java17.jep415.*;java.base/*;!*");
      ois.setObjectInputFilter(filter1);

      try {
          Object obj = ois.readObject();
          System.out.println("Read obj: " + obj);
      } catch (ClassNotFoundException e) {
          e.printStackTrace();
      }
  }

  private static byte[] convertObjectToStream(Object obj) {
      ByteArrayOutputStream boas = new ByteArrayOutputStream();
      try (ObjectOutputStream ois = new ObjectOutputStream(boas)) {
          ois.writeObject(obj);
          return boas.toByteArray();
      } catch (IOException ioe) {
          ioe.printStackTrace();
      }
      throw new RuntimeException();
  }

} 
```

输出

Terminal

```java
 Read obj: running ddos...! 
```

下面的例子将拒绝包`com.mkyong.java17.jep415.*`中的所有类:

```java
 byte[] bytes = convertObjectToStream(new DdosExample());

  ObjectInputFilter filter1 =
        ObjectInputFilter.Config.createFilter(
          "!com.mkyong.java17.jep415.*;java.base/*;!*"); 
```

重新运行；这一次，我们将无法反序列化该对象。

Terminal

```java
 Exception in thread "main" java.io.InvalidClassException: filter status: REJECTED
    at java.base/java.io.ObjectInputStream.filterCheck(ObjectInputStream.java:1412) 
```

14.2 下面的例子创建了一个反序列化过滤器来拒绝所有扩展了`JComponent`的类。

JComponentExample.java

```java
 package com.mkyong.java17.jep415;

import javax.swing.*;
import java.io.Serializable;

public class JComponentExample extends JComponent implements Serializable {
} 
```

JEP290_B.java

```java
 package com.mkyong.java17.jep415;

import javax.swing.*;
import java.io.*;

public class JEP290_B {

    public static void main(String[] args) throws IOException {

        byte[] bytes = convertObjectToStream(new JComponentExample());
        InputStream is = new ByteArrayInputStream(bytes);
        ObjectInputStream ois = new ObjectInputStream(is);

        ois.setObjectInputFilter(createObjectFilter());

        try {
            Object obj = ois.readObject();
            System.out.println("Read obj: " + obj);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    // reject all JComponent classes
    private static ObjectInputFilter createObjectFilter() {
        return filterInfo -> {
            Class<?> clazz = filterInfo.serialClass();
            if (clazz != null) {
                return (JComponent.class.isAssignableFrom(clazz))
                        ? ObjectInputFilter.Status.REJECTED
                        : ObjectInputFilter.Status.ALLOWED;
            }
            return ObjectInputFilter.Status.UNDECIDED;
        };
    }

    private static byte[] convertObjectToStream(Object obj) {
        ByteArrayOutputStream boas = new ByteArrayOutputStream();
        try (ObjectOutputStream ois = new ObjectOutputStream(boas)) {
            ois.writeObject(obj);
            return boas.toByteArray();
        } catch (IOException ioe) {
            ioe.printStackTrace();
        }
        throw new RuntimeException();
    }

} 
```

输出

Terminal

```java
 Exception in thread "main" java.io.InvalidClassException: filter status: REJECTED
at java.base/java.io.ObjectInputStream.filterCheck(ObjectInputStream.java:1412)
at java.base/java.io.ObjectInputStream.readNonProxyDesc(ObjectInputStream.java:2053)
at java.base/java.io.ObjectInputStream.readClassDesc(ObjectInputStream.java:1907)
at java.base/java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:2209)
at java.base/java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1742)
at java.base/java.io.ObjectInputStream.readObject(ObjectInputStream.java:514)
at java.base/java.io.ObjectInputStream.readObject(ObjectInputStream.java:472)
at com.mkyong.java17.jep415.JEP290_B.main(JEP290_B.java:17) 
```

14.3 Java 17 为`ObjectInputFilter`接口增加了`allowFilter`和`rejectFilter`，可以更快地创建反序列化过滤器。

```java
 allowFilter(Predicate<Class<?>>, ObjectInputFilter.Status)

  rejectFilter(Predicate<Class<?>>, ObjectInputFilter.Status) 
```

对于上面 14.2 中的例子，现在我们可以像下面这样重构代码:

```java
 // Java 9
    private static ObjectInputFilter createObjectFilter() {
        return filterInfo -> {
            Class<?> clazz = filterInfo.serialClass();
            if (clazz != null) {
                return (JComponent.class.isAssignableFrom(clazz))
                        ? ObjectInputFilter.Status.REJECTED
                        : ObjectInputFilter.Status.ALLOWED;
            }
            return ObjectInputFilter.Status.UNDECIDED;
        };
    }

    // Java 17
    // reject all JComponent classes
    ObjectInputFilter jComponentFilter = ObjectInputFilter.rejectFilter(
            JComponent.class::isAssignableFrom,
            ObjectInputFilter.Status.UNDECIDED);
    ois.setObjectInputFilter(jComponentFilter); 
```

14.4 回到 Java 17，这个 JEP 415 引入了一个过滤器工厂的概念，一个`BinaryOperator`，来动态地或上下文相关地选择不同的反序列化过滤器。工厂决定如何组合两个过滤器或更换过滤器。

下面是结合两个反序列化过滤器的 Java 17 过滤器工厂示例。

JEP415_B.java

```java
 package com.mkyong.java17.jep415;

import java.io.*;
import java.util.function.BinaryOperator;

public class JEP415_B {

  static class PrintFilterFactory implements BinaryOperator<ObjectInputFilter> {

      @Override
      public ObjectInputFilter apply(
              ObjectInputFilter currentFilter, ObjectInputFilter nextFilter) {

          System.out.println("Current filter: " + currentFilter);
          System.out.println("Requested filter: " + nextFilter);

          // Returns a filter that merges the status of a filter and another filter
          return ObjectInputFilter.merge(nextFilter, currentFilter);

          // some logic and return other filters
          // reject all JComponent classes
          /*return filterInfo -> {
              Class<?> clazz = filterInfo.serialClass();
              if (clazz != null) {
                  if(JComponent.class.isAssignableFrom(clazz)){
                      return ObjectInputFilter.Status.REJECTED;
                  }
              }
              return ObjectInputFilter.Status.ALLOWED;
          };*/

      }
  }

  public static void main(String[] args) throws IOException {

      // Set a filter factory
      PrintFilterFactory filterFactory = new PrintFilterFactory();
      ObjectInputFilter.Config.setSerialFilterFactory(filterFactory);

      // create a maxdepth and package filter
      ObjectInputFilter filter1 =
              ObjectInputFilter.Config.createFilter(
                  "com.mkyong.java17.jep415.*;java.base/*;!*");
      ObjectInputFilter.Config.setSerialFilter(filter1);

      // Create a filter to allow String.class only
      ObjectInputFilter intFilter = ObjectInputFilter.allowFilter(
              cl -> cl.equals(String.class), ObjectInputFilter.Status.REJECTED);

      // if pass anything other than String.class, hits filter status: REJECTED
      //byte[] byteStream =convertObjectToStream(99);

      // Create input stream
      byte[] byteStream =convertObjectToStream("hello");
      InputStream is = new ByteArrayInputStream(byteStream);
      ObjectInputStream ois = new ObjectInputStream(is);

      ois.setObjectInputFilter(intFilter);

      try {
          Object obj = ois.readObject();
          System.out.println("Read obj: " + obj);
      } catch (ClassNotFoundException e) {
          e.printStackTrace();
      }
  }

  private static byte[] convertObjectToStream(Object obj) {
      ByteArrayOutputStream boas = new ByteArrayOutputStream();
      try (ObjectOutputStream ois = new ObjectOutputStream(boas)) {
          ois.writeObject(obj);
          return boas.toByteArray();
      } catch (IOException ioe) {
          ioe.printStackTrace();
      }
      throw new RuntimeException();
  }

} 
```

输出

Terminal

```java
 Current filter: null
Requested filter: com.mkyong.java17.jep415.*;java.base/*;!*
Current filter: com.mkyong.java17.jep415.*;java.base/*;!*
Requested filter: predicate(
    com.mkyong.java17.jep415.JEP415_B$$Lambda$22/0x0000000800c01460@15aeb7ab,
      ifTrue: ALLOWED, ifFalse:REJECTED)
Read obj: hello 
```

**延伸阅读**

请阅读以下链接，了解更多反序列化过滤器示例:

*   [JEP 415:上下文相关的反序列化过滤器](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/415)
*   [JEP 290:过滤输入的串行化数据](http://web.archive.org/web/20221230032425/https://openjdk.java.net/jeps/290)
*   [序列化过滤](http://web.archive.org/web/20221230032425/https://docs.oracle.com/en/java/javase/17/core/serialization-filtering1.html)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221230032425/https://github.com/mkyong/core-java)

$ cd java-17

## 参考文献

*   [OpenJDK 17 项目](http://web.archive.org/web/20221230032425/https://openjdk.java.net/projects/jdk/17/)
*   [OpenJDK 源代码](http://web.archive.org/web/20221230032425/https://github.com/openjdk/)
*   [维基百科–SSE2(流媒体 SIMD 扩展 2)](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/SSE2)
*   [维基百科–strictfp](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Strictfp)
*   [维基百科–伪随机数发生器(PRNG)](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Pseudorandom_number_generator)
*   [Java 17 的增强伪随机数生成器](http://web.archive.org/web/20221230032425/https://mbien.dev/blog/entry/enhanced-pseudo-random-number-generators)
*   [苹果金属框架](http://web.archive.org/web/20221230032425/https://developer.apple.com/metal/)
*   [维基百科–苹果 M1](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Apple_M1)
*   【Java 的数据类和密封类型
*   [更简单的记录序列化](http://web.archive.org/web/20221230032425/https://inside.java/2021/03/12/simpler-serilization-with-records/)
*   [记录序列化](http://web.archive.org/web/20221230032425/https://inside.java/2020/07/20/record-serialization/)
*   [Java 中的自动矢量化](http://web.archive.org/web/20221230032425/http://daniel-strecker.com/blog/2020-01-14_auto_vectorization_in_java/)
*   [矢量处理器](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Vector_processor)
*   [标量处理器](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Scalar_processor)
*   [迈向更好的系列化](http://web.archive.org/web/20221230032425/https://cr.openjdk.java.net/~briangoetz/amber/serialization.html)
*   【Java 的模式匹配
*   [Java 版本历史](http://web.archive.org/web/20221230032425/https://en.wikipedia.org/wiki/Java_version_history)
*   [序列化过滤](http://web.archive.org/web/20221230032425/https://docs.oracle.com/en/java/javase/17/core/serialization-filtering1.html)
*   [OWASP–不可信数据的反序列化](http://web.archive.org/web/20221230032425/https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data)
*   [Brian Goetz——迈向更好的序列化](http://web.archive.org/web/20221230032425/https://cr.openjdk.java.net/~briangoetz/amber/serialization.html)
*   [Java 序列化和反序列化示例](http://web.archive.org/web/20221230032425/https://mkyong.com/java/java-serialization-examples/)

<input type="hidden" id="mkyong-current-postId" value="17013">