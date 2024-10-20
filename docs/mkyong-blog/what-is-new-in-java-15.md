# Java 15 的新特性是什么

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/what-is-new-in-java-15/>

![Java 15 logo](img/badc0719bd97275b32431283d33e266a.png)

[Java 15](http://web.archive.org/web/20221225035437/https://openjdk.java.net/projects/jdk/15/) 于 2020 年 9 月 15 日正式上市，[点击此处](http://web.archive.org/web/20221225035437/https://jdk.java.net/15/)下载 Java 15。

Java 15 特性。

*   [1。JEP 339:爱德华兹曲线数字签名算法(EdDSA)](#jep-339-edwards-curve-digital-signature-algorithm-eddsa)
*   [2。JEP 360:密封类(预览)](#jep-360-sealed-classes-preview)
*   [3。JEP 371:隐藏类](#jep-371hidden-classes)
*   [4。JEP 372:移除 Nashorn JavaScript 引擎](#jep-372remove-the-nashorn-javascript-engine)
*   [5。JEP 373:重新实现传统的 DatagramSocket API](#jep-373reimplement-the-legacy-datagramsocket-api)
*   [6。JEP 374:禁用和反对偏向锁](#jep-374disable-and-deprecate-biased-locking)
*   7 .[。JEP 375:实例的模式匹配(第二次预览)](#jep-375pattern-matching-for-instanceof-second-preview)
*   [8。JEP 377:ZGC:可扩展的低延迟垃圾收集器](#jep-377zgc-a-scalable-low-latency-garbage-collector)
*   [9。JEP 378:文本块](#jep-378text-blocks)
*   10。JEP 379:Shenandoah:一个低暂停时间的垃圾收集器
*   [11。JEP 381:移除 Solaris 和 SPARC 端口](#jep-381remove-the-solaris-and-sparc-ports)
*   [12。JEP 383:外部存储器访问 API(第二孵化器)](#jep-383foreign-memory-access-api-second-incubator)
*   13。JEP 384:记录(第二次预览)
*   [14。JEP 385:反对为移除激活 RMI](#jep-385deprecate-rmi-activation-for-removal)

*Java 15 开发者特性。*

密封类型(预览)、记录(第二次预览)、模式匹配(第二次预览)、隐藏类、文本块或多行(标准)、外部内存访问 API(第二个孵化器)。

**注**
[最新 Java 16](http://web.archive.org/web/20221225035437/https://mkyong.com/java/what-is-new-in-java-16/) 有什么新内容。

## 1。JEP 339:爱德华兹曲线数字签名算法(EdDSA)

密码学相关的东西，Java 15 使用 [Edwards-Curve 数字签名算法(EdDSA)](http://web.archive.org/web/20221225035437/https://en.wikipedia.org/wiki/EdDSA) 实现了一个额外的数字签名方案，如 [RFC 8032](http://web.archive.org/web/20221225035437/https://tools.ietf.org/html/rfc8032) 所述。EdDSA 签名方案因其比其他签名方案具有更高的安全性和性能(更快)而广受欢迎，也是 [TLS 1.3](http://web.archive.org/web/20221225035437/https://www.cloudflare.com/learning-resources/tls-1-3/) 中允许的签名方案之一。

回顾 Java 15 [签名算法](http://web.archive.org/web/20221225035437/https://docs.oracle.com/en/java/javase/15/docs/specs/security/standard-names.html#signature-algorithms)。

![EdDSA digital schema](img/ad990c4a5f0fb9c83b07f305c7c50d25.png)

示例代码。

```java
 package com.mkyong.java15.jep339;

import java.nio.charset.StandardCharsets;
import java.security.*;
import java.util.Base64;

public class JEP339 {

    public static void main(String[] args)
        throws NoSuchAlgorithmException, InvalidKeyException, SignatureException {

        KeyPairGenerator kpg = KeyPairGenerator.getInstance("Ed25519");
        KeyPair kp = kpg.generateKeyPair();

        byte[] msg = "abc".getBytes(StandardCharsets.UTF_8);

        Signature sig = Signature.getInstance("Ed25519");
        sig.initSign(kp.getPrivate());
        sig.update(msg);
        byte[] s = sig.sign();

        System.out.println(Base64.getEncoder().encodeToString(s));

    }
} 
```

*延伸阅读*

*   [JEP 339:爱德华兹曲线数字签名算法(EdDSA)](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/339)

## 2。JEP 360:密封类(预览)

这个 JEP 引入了几个新的关键字，`sealed`，`non-seal`，`permits`来支持[密封类和接口](http://web.archive.org/web/20221225035437/https://cr.openjdk.java.net/~briangoetz/amber/datum.html)。密封的类和接口限制了谁可以成为子类型。

2.1 下面的`sealed`接口允许三个指定的子类来实现它。

```java
 public sealed interface Command
    permits LoginCommand, LogoutCommand, PluginCommand{
    //...
} 
```

2.2 对于不允许的类，它会抛出编译时错误:

```java
 public final class UnknownCommand implements Command {
  //...
} 
```

```java
 class is not allowed to extend sealed class: Command 
```

2.3 密封类必须有子类，每个允许的子类必须选择一个修饰符(密封的、非密封的、最终的)来描述它如何继续由它的超类发起的密封

`final`

```java
 // close, dun extends me
  public final class LoginCommand implements Command{
  } 
```

`sealed`

```java
 // another sealed class
  // sealed class must have subclasses
  public sealed class LogoutCommand implements Command
        permits LogoutAndDeleteCachedCommand {
  } 
```

```java
 // Sealed this class again if you want
  public final class LogoutAndDeleteCachedCommand extends LogoutCommand {
  } 
```

`non-sealed`

```java
 // open...up to you to play this
  // Create custom plugin by extending this class
  public non-sealed class PluginCommand implements Command {
  } 
```

**注意**你注意到关键词`non-sealed`了吗？我认为这是 Java 中第一个连字符关键字。但是，这是一个预览功能；在未来的版本中，该关键字可能会发生变化。

2.4 该密封类或允许子类与[模式匹配](http://web.archive.org/web/20221225035437/https://cr.openjdk.java.net/~briangoetz/amber/pattern-match.html)。

```java
 switch (command) {
      case LoginCommand:      // login
      case LogoutCommand:     // logout
      case PluginCommand:     // custom plugin
      // no default needed, only permits 3 sub-classes
  } 
```

*P.S 密封类在 Java 16 中有第二个预览， [JEP 397](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/397) 。*

*延伸阅读*

*   [JEP 360:密封类(预览)](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/360)
*   为什么我们允许一个密封类扩展到一个非密封类？

## 3。JEP 371:隐藏类

3.1 这个 JEP 引入了隐藏类，它们不可被发现，并且具有有限的生命周期(较短的生命周期)，对于在运行时动态生成类的开发人员来说很好。现在我们可以使用这个新的[Lookup::defineHiddenClass](http://web.archive.org/web/20221225035437/https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/invoke/MethodHandles.Lookup.html#defineHiddenClass(byte%5B%5D,boolean,java.lang.invoke.MethodHandles.Lookup.ClassOption...))API 从字节创建一个隐藏的类或接口。

3.2 示例代码使用`defineHiddenClass`从 Base64 编码的类创建一个隐藏类，并手动启动静态`lookup`方法。

LookupProxyTest.java

```java
 package com.mkyong.java15.jep371;

import java.lang.invoke.MethodHandle;
import java.lang.invoke.MethodHandles;
import java.lang.invoke.MethodType;
import java.util.Base64;

public class LookupProxyTest {

    //Here is the Base64 encoded class.
    /*
    package com.mkyong.java15.jep371;

    public class LookUpProxy{

        public static Integer lookup() {
            return 1;
        }
    }*/
    static final String CLASS_IN_BASE64 =
            "yv66vgAAADcAFQoABAANCgAOAA8HABAHABEBAAY8aW5pdD4BAAMoKV" +
            "YBAARDb2RlAQAPTGluZU51bWJlclRhYmxlAQAGbG9va3VwAQAVKClM" +
            "amF2YS9sYW5nL0ludGVnZXI7AQAKU291cmNlRmlsZQEAEExvb2tVcF" +
            "Byb3h5LmphdmEMAAUABgcAEgwAEwAUAQAkY29tL21reW9uZy9qYXZh" +
            "MTUvamVwMzcxL0xvb2tVcFByb3h5AQAQamF2YS9sYW5nL09iamVjdA" +
            "EAEWphdmEvbGFuZy9JbnRlZ2VyAQAHdmFsdWVPZgEAFihJKUxqYXZh" +
            "L2xhbmcvSW50ZWdlcjsAIQADAAQAAAAAAAIAAQAFAAYAAQAHAAAAHQ" +
            "ABAAEAAAAFKrcAAbEAAAABAAgAAAAGAAEAAAADAAkACQAKAAEABwAA" +
            "AB0AAQAAAAAABQS4AAKwAAAAAQAIAAAABgABAAAABgABAAsAAAACAAw=";

    public static void main(String[] args) throws Throwable {

        //byte[] array = Files.readAllBytes(
        //      Paths.get("/home/mkyong/test/LookUpProxy.class"));
        //String s = Base64.getEncoder().encodeToString(array);
        //System.out.println(s);

        testHiddenClass();

    }

    // create a hidden class and run its static method
    public static void testHiddenClass() throws Throwable {

        byte[] classInBytes = Base64.getDecoder().decode(CLASS_IN_BASE64);

        Class<?> proxy = MethodHandles.lookup()
                .defineHiddenClass(classInBytes,
                        true, MethodHandles.Lookup.ClassOption.NESTMATE)
                .lookupClass();

        // output: com.mkyong.java15.jep371.LookUpProxy/0x0000000800b94440
        System.out.println(proxy.getName());

        MethodHandle mh = MethodHandles.lookup().findStatic(proxy,
                "lookup",
                MethodType.methodType(Integer.class));

        Integer status = (Integer) mh.invokeExact();
        System.out.println(status);

    }

} 
```

输出

Terminal

```java
 com.mkyong.java15.jep371.LookUpProxy/0x0000000800b94440
1 
```

3.3 这个 JEP 也弃用了`Unsafe.defineAnonymousClass` API，并将其标记为将来移除。请不要再使用这个 API 了。

```java
 byte[] classInBytes = Base64.getDecoder().decode(CLASS_IN_BASE64);

  Field theUnsafe = sun.misc.Unsafe.class.getDeclaredField("theUnsafe");
  theUnsafe.setAccessible(true);
  sun.misc.Unsafe unsafe = (sun.misc.Unsafe)theUnsafe.get(null);

  // @Deprecated(since = "15", forRemoval = false)
  Class<?> proxy = unsafe.defineAnonymousClass(
          LookupProxyTest.class, classInBytes, null); 
```

*延伸阅读*

*   JEP 371:隐藏类

## 4。JEP 372:移除 Nashorn JavaScript 引擎

*   Java 8 [JEP 174](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/174) 引入了 Nashorn 作为 Rhino Javascript 引擎的替代品。
*   Java 11 [JEP 335](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/335) 弃用了 Nashorn JavaScript 引擎和`jjs`工具。
*   现在，Java 15 永久移除了 Nashorn JavaScript 引擎和`jjs`工具。

这个 JEP 还去掉了下面两个模块:

*   `jdk.scripting.nashorn`–包含`jdk.nashorn.api.scripting`和`jdk.nashorn.api.tree`包。
*   `jdk.scripting.nashorn.shell`–包含 jjs 工具。

*延伸阅读*

*   [JEP 372:移除 Nashorn JavaScript 引擎](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/372)

## 5。JEP 373:重新实现传统的 DatagramSocket API

*   Java 13 [JEP 353](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/353) 重新实现了遗留的套接字 API——`java.net.Socket`和`java.net.ServerSocket`。
*   这一次，Java 15 重新实现了遗留的 datagram socket API—`java.net.DatagramSocket`和`java.net.MulticastSocket`。

*延伸阅读*

*   [JEP 373:重新实现传统的 DatagramSocket API](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/373)

## 6。JEP 374:禁用和反对偏向锁

默认情况下，该 JEP 禁用并取消了[偏向锁定](http://web.archive.org/web/20221225035437/https://blogs.oracle.com/dave/biased-locking-in-hotspot)。在 Java 15 之前，缺省情况下总是启用偏置锁定，从而提高同步内容的性能。

旧的或遗留的 Java 应用程序使用类似于`Hashtable`和`Vector`的同步集合 API，有偏向的锁定可能会提高性能。现在，较新的 Java 应用程序一般使用非同步集合`HashMap`和`ArrayList`，偏向锁的性能增益现在一般没那么有用了。

然而，对于 Java 15，我们仍然可以通过使用`-XX:+UseBiasedLocking`来启用偏置锁定，但是它会提示 VM 警告不推荐使用的 API。

Terminal

```java
 # Java 15
$ java -XX:+UseBiasedLocking name

 OpenJDK 64-Bit Server VM warning: Option UseBiasedLocking was deprecated
in version 15.0 and will likely be removed in a future release. 
```

*延伸阅读*

*   [JEP 374:禁用和反对偏向锁](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/374)
*   [Java 中的堆栈溢出偏置锁定](http://web.archive.org/web/20221225035437/https://stackoverflow.com/questions/9439602/biased-locking-in-java)

## 7 .。JEP 375:实例的模式匹配(第二次预览)

Java 14 [JEP 305](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/305) 引入了[模式匹配](http://web.archive.org/web/20221225035437/https://cr.openjdk.java.net/~briangoetz/amber/pattern-match.html)作为预览特性。这个 JEP 是模式匹配的第二次预览，以获得额外的反馈，对 API 没有改变。

一个典型的`instanceof-and-cast`来检查对象的类型并转换它。

```java
 private static void print(Object obj) {

      if (obj instanceof String) {        // instanceof

          String s = (String) obj;        // cast

          if ("java15".equalsIgnoreCase(s)) {
              System.out.println("Hello Java 15");
          }

      } else {
          System.out.println("not a string");
      }

  } 
```

对于模式匹配，我们可以在一行中进行检查、造型和绑定。

```java
 private static void printWithPatternMatching(Object obj) {

      // instanceof, cast and bind variable in one line.
      if (obj instanceof String s) {         

          if ("java15".equalsIgnoreCase(s)) {
              System.out.println("Hello Java 15");
          }

      } else {
          System.out.println("not a string");
      }

  } 
```

模式匹配是 Java 16 的标准特性， [JEP 394](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/394) 。

*延伸阅读*

*   [JEP 375:实例的模式匹配(第二次预览)](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/375)

## 8。JEP 377:ZGC:可扩展的低延迟垃圾收集器

Java 11 [JEP 333](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/333) 引入了 [ZGC 垃圾收集器](http://web.archive.org/web/20221225035437/https://wiki.openjdk.java.net/display/zgc/Main)作为实验特性。

*   这个 JEP 修复了一些错误，增加了一些功能和增强，现在支持主要平台，如 Linux/x86_64、Linux/aarch64、Windows 和 macOS。
*   这个 JEP 还将 Z 垃圾收集器从一个实验性特性变成了一个产品特性。然而，默认的垃圾收集器仍然是 [G1](http://web.archive.org/web/20221225035437/https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html) 。

下面的命令启用 ZGC 垃圾收集器。

Terminal

```java
 $ java -XX:+UseZGC className 
```

*延伸阅读*

*   JEP 377:ZGC:一个可扩展的低延迟垃圾收集器

## 9。JEP 378:文本块

最后，多行字符串或文本块是 Java 15 的一个永久特性。

历史:

*   Java 12 [JEP 326 原始字符串文字](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/326)
*   Java 13 [JEP 355:文本块(预览)](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/355)
*   Java 14 [JEP 368:文本块(第二次预览)](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/368)
*   Java 15，永久特性。

示例代码。

```java
 String html = "<html>\n" +
                "   <body>\n" +
                "      <p>Hello, World</p>\n" +
                "   </body>\n" +
                "</html>\n";

  String java15 = """
                  <html>
                      <body>
                          <p>Hello, World</p>
                      </body>
                  </html>
                  """; 
```

*延伸阅读*

*   [JEP 378:文本块](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/378)

## 10。JEP 379:Shenandoah:一个低暂停时间的垃圾收集器

Java 12 [JEP 189](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/189) 引入了 [Shenandoah 垃圾收集器](http://web.archive.org/web/20221225035437/https://wiki.openjdk.java.net/display/shenandoah/Main)作为实验特性，现在成为 Java 15 中的产品特性。

在 Java 15 之前，我们需要`-XX:+UnlockExperimentalVMOptions -XX:+UseShenandoahGC`来启用 Shenandoah GC。

Terminal

```java
 java -XX:+UnlockExperimentalVMOptions -XX:+UseShenandoahGC 
```

在 Java 15 中，我们只需要`-XX:+UseShenandoahGC`来启用 Shenandoah GC。

Terminal

```java
 java -XX:+UseShenandoahGC 
```

然而，官方的 [OpenJDK 15](http://web.archive.org/web/20221225035437/https://jdk.java.net/15/) 构建并不包括 Shenandoah GC(就像 Java 12 中发生的一样)。阅读这个故事——[不是所有的 OpenJDK 12 版本都包含 Shenandoah:下面是原因](http://web.archive.org/web/20221225035437/https://developers.redhat.com/blog/2019/04/19/not-all-openjdk-12-builds-include-shenandoah-heres-why/)。

Terminal

```java
 $ java -XX:+UseShenandoahGC ClassName

  Error occurred during initialization of VM
  Option -XX:+UseShenandoahGC not supported

$ java -version

  openjdk version "15" 2020-09-15
  OpenJDK Runtime Environment (build 15+36-1562)
  OpenJDK 64-Bit Server VM (build 15+36-1562, mixed mode, sharing) 
```

为了尝试 Shenandoah GC，我们需要其他 JDK 版本，如 [AdoptOpenJDK](http://web.archive.org/web/20221225035437/https://adoptopenjdk.net/) 。

Terminal

```java
 $ java -XX:+UseShenandoahGC ClassName

$ java -version

  openjdk version "15" 2020-09-15
  OpenJDK Runtime Environment AdoptOpenJDK (build 15+36)
  OpenJDK 64-Bit Server VM AdoptOpenJDK (build 15+36, mixed mode, sharing) 
```

默认的垃圾收集器仍然是 G1 T2。

*延伸阅读*

*   [JEP 379:谢南多厄:一个低暂停时间的垃圾收集器](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/379)

## 11。JEP 381:移除 Solaris 和 SPARC 端口

Java 14 [JEP 362](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/362) 弃用了 Solaris/SPARC、Solaris/x64 和 Linux/SPARC 端口，现在它在 Java 15 中被正式删除。

*延伸阅读*

*   [JEP 381:移除 Solaris 和 SPARC 端口](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/381)

## 12。JEP 383:外部存储器访问 API(第二孵化器)

Java 14 [JEP 370](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/370) 引入了新的外来内存访问 API 作为[孵化器模块](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/11)。这个 JEP 对 API 做了一些修改，它仍然会在孵化器模块中。

示例代码

HelloForeignMemory.java

```java
 import jdk.incubator.foreign.MemoryAddress;
import jdk.incubator.foreign.MemoryHandles;
import jdk.incubator.foreign.MemorySegment;

import java.lang.invoke.VarHandle;
import java.nio.ByteOrder;

public class HelloForeignMemory {

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

我们需要添加`--add-modules jdk.incubator.foreign`来启用孵化器模块`jdk.incubator.foreign`。

Terminal

```java
 $ javac --add-modules jdk.incubator.foreign HelloForeignMemory.java

warning: using incubating module(s): jdk.incubator.foreign
1 warning

$ java --add-modules jdk.incubator.foreign HelloForeignMemory

WARNING: Using incubator modules: jdk.incubator.foreign
MemoryAddress{ region: MemorySegment{ id=0x27c908f5 limit: 1024 } offset=0x0 }
999 
```

*延伸阅读*

*   [JEP 383:外来存储器访问 API(第二孵化器)](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/383)

## 13。JEP 384:记录(第二次预览)

Java 14 [JEP 359](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/384) 引入了记录作为预览功能。这个 JEP 用支持密封类型、本地记录、记录注释和记录反射 API 等特性增强了记录。

13.1 记录和密封类型

Fruit.java

```java
 public sealed interface Fruit permits Apple, Orange {}

record Apple() implements Fruit{}
record Orange() implements Fruit{} 
```

13.2 本地记录-方法内部声明的记录。

下面的代码片段使用本地记录来提高流操作的可读性。

```java
 List<Merchant> findTopMerchants(List<Merchant> merchants, int month) {

      // Local record
      record MerchantSales(Merchant merchant, double sales) {
      }

      return merchants.stream()
              .map(merchant -> new MerchantSales(merchant, computeSales(merchant, month)))
              .sorted((m1, m2) -> Double.compare(m2.sales(), m1.sales()))
              .map(MerchantSales::merchant)
              .collect(toList());
  }

  private double computeSales(Merchant merchant, int month) {
      // some business logic to get the sales...
      return ThreadLocalRandom.current().nextDouble(100, 10000);
  } 
```

13.3 记录注释

```java
 public record Game(@CustomAnno Rank rank) { ... } 
```

13.4 反射 API

`java.lang.Class`增加了两个公共方法:

*   `RecordComponent[] getRecordComponents()`
*   `boolean isRecord()`

记录是 Java 16 中的标准特性， [JEP 395](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/384) 。

*延伸阅读*

*   [JEP 384:记录(第二次预览)](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/384)

## 14。JEP 385:反对为移除激活 RMI

这个 JEP 反对过时的 RMI 激活机制。这不会影响马绍尔群岛的其他部分。

*延伸阅读*

*   [JEP 385:反对为移除激活 RMI](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/385)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221225035437/https://github.com/mkyong/core-java)

$ cd java-15

## 参考文献

*   [OpenJDK 15 项目](http://web.archive.org/web/20221225035437/https://openjdk.java.net/projects/jdk/15/)
*   [cloud flare–TLS 1.3 有什么新功能？](http://web.archive.org/web/20221225035437/https://www.cloudflare.com/learning-resources/tls-1-3/)
*   [维基百科–爱德华兹曲线数字签名算法(EdDSA)](http://web.archive.org/web/20221225035437/https://en.wikipedia.org/wiki/EdDSA)
*   [RFC 8032](http://web.archive.org/web/20221225035437/https://tools.ietf.org/html/rfc8032)
*   【Java 的模式匹配
*   [热点偏锁](http://web.archive.org/web/20221225035437/https://blogs.oracle.com/dave/biased-locking-in-hotspot)
*   [open JDK Wiki–ZCG](http://web.archive.org/web/20221225035437/https://wiki.openjdk.java.net/display/zgc/Main)
*   [Oracle–Z 垃圾收集器](http://web.archive.org/web/20221225035437/https://docs.oracle.com/en/java/javase/11/gctuning/z-garbage-collector1.html)
*   [G1 垃圾收集工](http://web.archive.org/web/20221225035437/https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html)
*   [谢南多厄垃圾收集器](http://web.archive.org/web/20221225035437/https://wiki.openjdk.java.net/display/shenandoah/Main)
*   [JEP 11 号:孵化器模块](http://web.archive.org/web/20221225035437/https://openjdk.java.net/jeps/11)
*   [Java 版本历史](http://web.archive.org/web/20221225035437/https://en.wikipedia.org/wiki/Java_version_history)

<input type="hidden" id="mkyong-current-postId" value="16189">