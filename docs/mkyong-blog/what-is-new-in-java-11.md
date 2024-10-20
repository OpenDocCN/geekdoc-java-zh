# Java 11 的新特性是什么

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/what-is-new-in-java-11/>

![Java 11 logo](img/f3a2fce1e221693e84ee93f6722fa585.png)

[Java 11](http://web.archive.org/web/20221230032428/https://openjdk.java.net/projects/jdk/11/) 于 2018 年 9 月 25 日正式发布，这是一个长期支持(LTS)版本，[在此下载 Java 11](http://web.archive.org/web/20221230032428/https://jdk.java.net/java-se-ri/11)或此 [openJDK 存档](http://web.archive.org/web/20221230032428/https://jdk.java.net/archive/)。

Java 11 的特性。

*   [1。JEP 181:嵌套式访问控制](#jep-181-nest-based-access-control)
*   [2。JEP 309:动态类文件常量](#jep-309-dynamic-class-file-constants)
*   [3。JEP 315:改进 Aarch64 内部函数](#jep-315-improve-aarch64-intrinsics)
*   [4。JEP 318:ε:一个无操作垃圾收集器(实验)](#jep-318-epsilon-a-no-op-garbage-collector-experimental)
*   [5。JEP 320:移除 Java EE 和 CORBA 模块](#jep-320-remove-the-java-ee-and-corba-modules)
*   [6。JEP 321: HTTP 客户端(标准)](#jep-321-http-client-standard)
*   [7。JEP 323:Lambda 参数的局部变量语法](#jep-323-local-variable-syntax-for-lambda-parameters)
*   [8。JEP 324:与曲线 25519 和曲线 448](#jep-324-key-agreement-with-curve25519-and-curve448) 的关键协议
*   [9。JEP 327: Unicode 10](#jep-327-unicode-10)
*   10。JEP 328:飞行记录器
*   [11。JEP 329: ChaCha20 和 Poly1305 密码算法](#jep-329-chacha20-and-poly1305-cryptographic-algorithms)
*   [12。JEP 330:启动单文件源代码程序](#jep-330-launch-single-file-source-code-programs)
*   13。JEP 331:低开销堆分析
*   [14。JEP 332:传输层安全性(TLS) 1.3](#jep-332-transport-layer-security-tls-13)
*   15。JEP 333: ZGC:一个可扩展的低延迟垃圾收集器(实验)
*   16。JEP 335:反对 Nashorn JavaScript 引擎
*   [17。JEP 336:反对 Pack200 工具和 API](#jep-336-deprecate-the-pack200-tools-and-api)

*Java 11 开发者特性。*

新增 HTTP 客户端 API`java.net.http.*`，lambda 参数中的`var`，Java fright recorder (JFR)，启动单文件程序`java ClassName.java`，支持 ChaCha20 密码算法。

**最新 JDK 发布 Java 16**
最新 Java 16 有什么新内容。

## 1。JEP 181:嵌套式访问控制

它直接支持嵌套成员内部的私有访问，不再需要通过自动生成的桥方法`access$000`。此外，新的验证嵌套 API 允许嵌套成员内部的私有反射访问。

Java 11 之前。

.java

```java
 public class Alphabet {
    private String name = "I'm Alphabet!";

    public class A {
        public void printName() {
            System.out.println(name);       // access Alphabet's private member!
        }
    }
} 
```

如果我们编译上面的类，它将生成两个类，`Alphabet`和`Alphabet$A`，即使是嵌套类也是一个典型的具有唯一名称的类。JVM 访问规则不允许不同类中的私有访问。然而，Java 允许嵌套成员内部的私有访问，所以 Java 编译器创建了一个桥方法`access$000`来应用于 JVM 访问规则。

```java
 // After javac Alphabet.java, Java compiler created something similar to this.
public class Alphabet {
  private String name = "I'm Alphabet!";
  String access$000(){
    return name;
  }
}

public class Alphabet$A {
  final Alphabet obj;
  public void printName(){
    System.out.println(obj.access$000());
  }
} 
```

在 Java 11 中，Java 编译器不会为嵌套成员内的私有访问生成任何桥方法`access$000`。这个新的 JVM 访问规则，基于嵌套的访问控制允许嵌套成员内部的私有访问。

*延伸阅读*

*   JEP 181:基于嵌套的访问控制
*   [Java 11–基于嵌套的访问控制](/web/20221230032428/https://mkyong.com/java/java-11-nest-based-access-control/)

## 2。JEP 309:动态类文件常量

扩展了类文件格式，以支持新的常量池形式，`CONSTANT_Dynamic`，目标语言设计者和编译器实现者。

*延伸阅读*

*   [JEP 309:动态类文件常量](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/309)

## 3。JEP 315:改进 Aarch64 内部函数

优化了现有的字符串和数组内函数，在 Arm64 或 Aarch64 处理器上为`Math.sin()`、`Math.cos()`和`Match.log()`实现了新的内函数。意味着更好的性能。

*P.S 一个[内在](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/Intrinsic_function)用于利用 CPU 架构特定的汇编代码来提高性能。*

*延伸阅读*

*   [JEP 315:改进 Aarch64 的内部函数](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/315)

## 4。JEP 318:ε:一个无操作垃圾收集器(实验)

一个新的 No-Op(无操作)垃圾收集器，它分配内存但不会收集任何垃圾(内存分配)，一旦 Java 堆耗尽，JVM 就会关闭。

一些使用案例:

*   性能试验
*   虚拟机接口测试
*   短暂的工作

这个 GC 是一个实验性的特性；我们需要使用以下选项来启用新的 Epsilon GC。

```java
 -XX:+UnlockExperimentalVMOptions -XX:+UseEpsilonGC 
```

*延伸阅读*

*   [JEP 318:ε:一个无操作垃圾收集器(实验)](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/318)

## 5。JEP 320:移除 Java EE 和 CORBA 模块

Java 9 弃用了下面的 Java EE 和 CORBA 模块，现在在 Java 11 中被删除了。如果您想迁移到 Java 11，请确保您的项目没有使用以下任何包或工具。

移除的包:

*   java.xml.ws (JAX)
*   java.xml.bind
*   java.activation (JAF)
*   java.xml.ws.annotation(通用注释)
*   java.corba
*   java .事务(JTA)
*   java.se.ee(上述六个模块的聚合器模块)

移除的工具:

*   wsgen 和 wsimport(来自 jdk.xml.ws)
*   schemagen 和 xjc(来自 jdk.xml.bind)
*   idlj、orbd、servertool 和 tnamesrv(来自 java.corba)

*延伸阅读*

*   [JEP 320:移除 Java EE 和 CORBA 模块](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/320)

## 6。JEP 321: HTTP 客户端(标准)

`java.net.http`包中的这个`HTTP Client API`在 Java 9 中引入，在 Java 10 中更新，现在是 Java 11 中的标准特性。

Java 11 `HttpClient`发送一个简单的`GET`请求。

```java
 HttpClient httpClient = HttpClient.newBuilder()
            .version(HttpClient.Version.HTTP_1_1)
            .connectTimeout(Duration.ofSeconds(10))
            .build();

    HttpRequest request = HttpRequest.newBuilder()
            .GET()
            .uri(URI.create("https://httpbin.org/get"))
            .setHeader("User-Agent", "Java 11 HttpClient Bot")
            .build();

    HttpResponse<String> response =
      httpClient.send(request, HttpResponse.BodyHandlers.ofString());

    HttpHeaders headers = response.headers();
    headers.map().forEach((k, v) -> System.out.println(k + ":" + v));

    System.out.println(response.statusCode());
    System.out.println(response.body()); 
```

*延伸阅读*

*   [JEP 321: HTTP 客户端(标准)](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/321)
*   [Java 11 HttpClient 示例](/web/20221230032428/https://mkyong.com/java/java-11-httpclient-examples/)

## 7。JEP 323:Lambda 参数的局部变量语法

这个 JEP 增加了对 lambda 参数中关键字`var`的支持。

```java
 List<String> list = Arrays.asList("a", "b", "c");
  String result = list.stream()
          .map((var x) -> x.toUpperCase())
          .collect(Collectors.joining(","));
  System.out.println(result2); 
```

然而，lambda 可以进行类型推断；上面的例子相当于这个:

```java
 List<String> list = Arrays.asList("a", "b", "c");
  String result = list.stream()
          .map(x -> x.toUpperCase())
          .collect(Collectors.joining(",")); 
```

那么，这个 JEP 为什么要在 lambda 参数中加入`var`？好处是现在我们可以向 lambda 参数添加注释，参见以下示例:

```java
 import org.jetbrains.annotations.NotNull;

  List<String> list = Arrays.asList("a", "b", "c", null);
  String result = list.stream()
          .map((@NotNull var x) -> x.toUpperCase())
          .collect(Collectors.joining(","));
  System.out.println(result3); 
```

*延伸阅读*

*   [JEP 323:Lambda 参数的局部变量语法](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/323)

## 8。JEP 324:与曲线 25519 和曲线 448
的关键协议

Java 密码学相关项目。它用 [Curve25519](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/Curve25519) 和 [Curve448](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/Curve448) 算法取代了现有的椭圆曲线 Diffie-Hellman (ECDH)方案，这是一种在 [RFC 7748](http://web.archive.org/web/20221230032428/https://tools.ietf.org/html/rfc7748) 中定义的密钥协商方案。

使用`Curve25519`算法生成密钥对的简单`KeyPairGenerator`示例。

GenerateKeyPairs.java

```java
 package com.mkyong.java11.jep324;

import java.nio.charset.StandardCharsets;
import java.security.*;
import java.security.spec.NamedParameterSpec;

public class GenerateKeyPairs {

    public static void main(String[] args) throws NoSuchAlgorithmException,
        InvalidAlgorithmParameterException {

        KeyPairGenerator kpg = KeyPairGenerator.getInstance("XDH");
        NamedParameterSpec paramSpec = new NamedParameterSpec("X25519");
        kpg.initialize(paramSpec); // equivalent to kpg.initialize(255)
        // alternatively: kpg = KeyPairGenerator.getInstance("X25519")
        KeyPair kp = kpg.generateKeyPair();

        System.out.println("--- Public Key ---");
        PublicKey publicKey = kp.getPublic();

        System.out.println(publicKey.getAlgorithm());   // XDH
        System.out.println(publicKey.getFormat());      // X.509

        // save this public key
        byte[] pubKey = publicKey.getEncoded();

        System.out.println("---");

        System.out.println("--- Private Key ---");
        PrivateKey privateKey = kp.getPrivate();

        System.out.println(privateKey.getAlgorithm());  // XDH
        System.out.println(privateKey.getFormat());     // PKCS#8

        // save this private key
        byte[] priKey = privateKey.getEncoded();
    }
} 
```

*延伸阅读*

*   [JEP 324:与 Curve25519 和 Curve448 的密钥协议](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/324)
*   [Oracle–Java 安全标准算法名称](http://web.archive.org/web/20221230032428/https://docs.oracle.com/en/java/javase/11/docs/specs/security/standard-names.html#keypairgenerator-algorithms)

## 9。JEP 327: Unicode 10

这意味着更多的代码点，更多的表情图标🙂下面的例子打印了一个 Unicode 码位。

PrintUnicode.java

```java
 package com.mkyong.java11;

public class PrintUnicode {

    public static void main(String[] args) {
        String codepoint = "U+1F92A";   // crazy face
        System.out.println(convertCodePoints(codepoint));
    }

    // Java, UTF-16
    // Convert code point to unicode
    static char[] convertCodePoints(String codePoint) {
        Integer i = Integer.valueOf(codePoint.substring(2), 16);
        char[] chars = Character.toChars(i);
        return chars;

    }

} 
```

*延伸阅读*

*   [JEP 327: Unicode 10](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/327)
*   [Unicode 10](http://web.archive.org/web/20221230032428/https://unicode.org/versions/Unicode10.0.0/)
*   [Unicode 10.0 的新功能](http://web.archive.org/web/20221230032428/https://blog.emojipedia.org/whats-new-in-unicode-10/)
*   [Java 中的 Unicode 乐趣](http://web.archive.org/web/20221230032428/https://www.codetab.org/post/java-unicode-basics/)

## 10。JEP 328:飞行记录器

Java 飞行记录器(JFR)是甲骨文 JDK 公司的商业产品，现在它在 OpenJDK 11 中是开源的。这个 JFR 是一个分析工具，用于诊断正在运行的 Java 应用程序。下面的命令在一个 Java 应用程序上开始一个 60 秒的 JFR 记录，将记录的数据转储到一个。jfr 的文件。

Termianl

```java
 $ java -XX:StartFlightRecording=duration=60s,settings=profile,filename=app.jfr MyHelloWorldApp 
```

那么如何处理`.jfr`文件呢？我们可以使用 [Java Mission Control (JMC)](http://web.archive.org/web/20221230032428/https://openjdk.java.net/projects/jmc/) 对`.jfr`文件进行分析和可视化。

*延伸阅读*

*   JEP 328:飞行记录器
*   [Java 飞行记录器示例](/web/20221230032428/https://mkyong.com/java/java-11-java-flight-recorder/)

## 11。JEP 329: ChaCha20 和 Poly1305 密码算法

`ChaCha20`是一种高速流密码，一种加密解密算法。`ChaCha20-Poly1305`是指`ChaCha20`在 AEAD 模式下运行，与`Poly1305`认证器、加密和认证一起，二者都在 [RFC 7539](http://web.archive.org/web/20221230032428/https://tools.ietf.org/html/rfc7539) 中定义。这个 JEP 更新的`ChaCha20`密码算法是不安全的 [RC4](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/RC4) 流密码的替代品。

`ChaCha20`的输入是:

*   256 位密钥(32 字节)
*   96 位随机数(12 字节)
*   32 位初始计数(4 字节)

```java
 Cipher cipher = Cipher.getInstance("ChaCha20");

    ChaCha20ParameterSpec param = new ChaCha20ParameterSpec(nonce, counter);

    cipher.init(Cipher.ENCRYPT_MODE, key, param);

    byte[] encryptedText = cipher.doFinal(pText); 
```

请参考本[Java 11–chacha 20 流密码示例](/web/20221230032428/https://mkyong.com/java/java-11-chacha20-stream-cipher-examples/)

`ChaCha20-Poly1305`的输入是:

*   256 位密钥(32 字节)
*   96 位随机数(12 字节)

```java
 Cipher cipher = Cipher.getInstance("ChaCha20-Poly1305");

   // IV, initialization value with nonce
   IvParameterSpec iv = new IvParameterSpec(nonce);

   cipher.init(Cipher.ENCRYPT_MODE, key, iv);

   byte[] encryptedText = cipher.doFinal(pText); 
```

*延伸阅读*

*   [JEP 329: ChaCha20 和 Poly1305 密码算法](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/329)
*   [Java 11–chacha 20-poly 1305 加密示例](/web/20221230032428/https://mkyong.com/java/java-11-chacha20-poly1305-encryption-examples/)

## 12。JEP 330:启动单文件源代码程序

这个单文件源代码程序意味着整个 Java 程序都在一个源代码`.java`文件中。这个 JEP 在学习 Java 的初期是一个友好的特性，但是在 Java 开发中没有太大的好处，我们都用 IDE。

查看单个文件源代码。

HelloJava.java

```java
 public class HelloJava {

    public static void main(String[] args) {

        System.out.println("Hello World!");

    }
} 
```

Java 11 之前。

Termianl

```java
 $ javac HelloJava.java

$ java HelloJava

Hello World! 
```

现在 Java 11。

Termianl

```java
 $ java HelloJava.java

Hello World! 
```

**Shebang #！**
现在，单个 Java 程序可以使用 Linux Shebang 作为脚本运行。

run.sh

```java
 #!/opt/java/openjdk/bin/java --source 11
public class SheBang {

    public static void main(String[] args) {

        System.out.println("Hello World!");

    }
} 
```

*延伸阅读*

*   JEP 330:启动单文件源代码程序
*   [Docker 中的 Java 11 shebang 示例](/web/20221230032428/https://mkyong.com/java/java-11-shebang-example-in-docker/)

## 13。JEP 331:低开销堆分析

Java 虚拟机工具接口(JVMTI)是在 J2SE 5、JDK 5 (Tiger)中引入的，它为分析或监控工具提供了访问 JVM 状态的 API。这个 JEP 在 JVMIT 中添加了新的低开销堆分析 API。

*延伸阅读*

*   [JEP 331:低开销堆分析](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/331)
*   [Oracle–JVM 工具接口](http://web.archive.org/web/20221230032428/https://docs.oracle.com/javase/8/docs/platform/jvmti/jvmti.html)

## 14。JEP 332:传输层安全性(TLS) 1.3

Java 11 支持 [RFC 8446](http://web.archive.org/web/20221230032428/https://tools.ietf.org/html/rfc8446) 传输层安全(TLS) 1.3 协议。然而，并不是所有的 TLS 1.3 特性都实现了，详情请参考这个 [JEP 332](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/332) 。

Java 安全套接字扩展(JSSE) + TLS 1.3 示例。

```java
 import javax.net.ssl.SSLSocket;
import javax.net.ssl.SSLSocketFactory;

SSLSocketFactory factory =
        (SSLSocketFactory) SSLSocketFactory.getDefault();
socket =
        (SSLSocket) factory.createSocket("google.com", 443);

socket.setEnabledProtocols(new String[]{"TLSv1.3"});
socket.setEnabledCipherSuites(new String[]{"TLS_AES_128_GCM_SHA256"}); 
```

*延伸阅读*

*   [JEP 332:传输层安全性(TLS) 1.3](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/332)
*   [Java SSLSocket TLS 1.3 示例](/web/20221230032428/https://mkyong.com/java/java-sslsocket-tls-1-3-example/)

## 15。JEP 333: ZGC:一个可扩展的低延迟垃圾收集器(实验)

Z 垃圾收集器(ZGC)是一个实验性的垃圾收集器；它具有不超过 10ms 低暂停时间。这种 ZCG 只在 Linux/64 上支持。

*   在 Java 14 中，这个 ZCG 扩展了对 macOS [JEP 364](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/364) 和 Windows [JEP 365](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/365) 的支持。
*   这个 ZGC 垃圾收集器是 Java 15 [JEP 377](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/377) 中的一个产品特性。

*延伸阅读*

*   [JEP 333: ZGC:一个可扩展的低延迟垃圾收集器(实验)](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/333)
*   [open JDK Wiki–ZCG](http://web.archive.org/web/20221230032428/https://wiki.openjdk.java.net/display/zgc/Main)
*   [Oracle–Z 垃圾收集器](http://web.archive.org/web/20221230032428/https://docs.oracle.com/en/java/javase/11/gctuning/z-garbage-collector1.html)

## 16。JEP 335:反对 Nashorn JavaScript 引擎

Nashorn JavaScript 脚本引擎和`jjs`工具已被弃用，可能会在未来的版本中删除。

*历史*

*   Java 8 [JEP 174](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/174) 引入了 Nashorn，作为 Rhino Javascript 引擎的替代品。
*   Java 11 [JEP 335](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/335) 弃用了 Nashorn。
*   Java 15 [JEP 372](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/372) 移除了 Nashorn JavaScript 引擎和 jjs 工具。

*延伸阅读*

*   [JEP 335:反对 Nashorn JavaScript 引擎](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/335)

## 17。JEP 336:反对 Pack200 工具和 API

这个 JEP 弃用了`pack200`和`unpack200`工具，以及`java.util.jar`包中的`Pack200` API，它可能会在未来的版本中移除。

JEP 367 移除了 Pack200 工具和 API。

*延伸阅读*

*   [JEP 336:反对 Pack200 工具和 API](http://web.archive.org/web/20221230032428/https://openjdk.java.net/jeps/336)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20221230032428/https://github.com/mkyong/core-java)

$ cd java-11

## 参考文献

*   [OpenJDK 11 项目](http://web.archive.org/web/20221230032428/https://openjdk.java.net/projects/jdk/11/)
*   [Java 版本历史](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/Java_version_history)
*   [Unicode 10](http://web.archive.org/web/20221230032428/https://unicode.org/versions/Unicode10.0.0/)
*   [维基百科–AAR ch 64 处理器](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/ARM_architecture)
*   [维基百科–内在](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/Intrinsic_function)
*   Java 安全性有什么新特性？
*   [Oracle–Java 安全标准算法名称](http://web.archive.org/web/20221230032428/https://docs.oracle.com/en/java/javase/11/docs/specs/security/standard-names.html#keypairgenerator-algorithms)
*   [RFC 7748](http://web.archive.org/web/20221230032428/https://tools.ietf.org/html/rfc7748)
*   [维基百科–curve 25519](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/Curve25519)
*   [维基百科–curve 448](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/Curve448)
*   [Java 飞行记录器](http://web.archive.org/web/20221230032428/https://mkyong.com/java/java-11-java-flight-recorder/)
*   [Java 任务控制(JMC)](http://web.archive.org/web/20221230032428/https://openjdk.java.net/projects/jmc/)
*   [RFC 7539](http://web.archive.org/web/20221230032428/https://tools.ietf.org/html/rfc7539)
*   [Java 11–Chipher](http://web.archive.org/web/20221230032428/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/javax/crypto/Cipher.html)
*   [ChaCha20](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/Salsa20#ChaCha_variant)
*   [RC4](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/RC4)
*   [Java 11–chacha 20 流密码示例](/web/20221230032428/https://mkyong.com/java/java-11-chacha20-stream-cipher-examples/)
*   [Java 11–chacha 20-poly 1305 加密示例](/web/20221230032428/https://mkyong.com/java/java-11-chacha20-poly1305-encryption-examples/)
*   [Docker 中的 Java 11 shebang 示例](/web/20221230032428/https://mkyong.com/java/java-11-shebang-example-in-docker/)
*   [维基百科–Java 虚拟机工具接口](http://web.archive.org/web/20221230032428/https://en.wikipedia.org/wiki/Java_Virtual_Machine_Tools_Interface)
*   [Oracle–JVM 工具接口](http://web.archive.org/web/20221230032428/https://docs.oracle.com/javase/8/docs/platform/jvmti/jvmti.html)
*   [Java SSLSocket TLS 1.3 示例](/web/20221230032428/https://mkyong.com/java/java-sslsocket-tls-1-3-example/)
*   [RFC 8446](http://web.archive.org/web/20221230032428/https://tools.ietf.org/html/rfc8446)
*   [Oracle–Java 安全开发人员指南](http://web.archive.org/web/20221230032428/https://docs.oracle.com/javase/10/security/toc.htm)
*   [Oracle–Z 垃圾收集器](http://web.archive.org/web/20221230032428/https://docs.oracle.com/en/java/javase/11/gctuning/z-garbage-collector1.html)
*   [open JDK Wiki–ZCG](http://web.archive.org/web/20221230032428/https://wiki.openjdk.java.net/display/zgc/Main)
*   [Oracle–Z 垃圾收集器](http://web.archive.org/web/20221230032428/https://docs.oracle.com/en/java/javase/11/gctuning/z-garbage-collector1.html)
*   [Java 11 HttpClient 示例](/web/20221230032428/https://mkyong.com/java/java-11-httpclient-examples/)

<input type="hidden" id="mkyong-current-postId" value="15725">