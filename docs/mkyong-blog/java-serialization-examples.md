# Java 序列化和反序列化示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-serialization-examples/>

在 Java 中，`Serialization`的意思是将 Java 对象转换成字节流；`Deserialization`表示将序列化对象的字节流转换回原来的 Java 对象。

目录。

*   [1。Hello World Java 序列化](#hello-world-java-serialization)
*   [2。Java . io . notserializableexception](#javaionotserializableexception)
*   [3。什么是 serialVersionUID？](#what-is-serialversionuid)
*   [4。什么是瞬变？](#what-is-transient)
*   [5。将对象序列化到文件](#serialize-object-to-file)
*   [6。为什么需要 Java 中的序列化？](#why-need-serialization-in-java)
*   7 .[。不可信数据的反序列化](#deserialization-of-untrusted-data)
    *   [7.1 反序列化和堆栈溢出](#deserialization-and-stackoverflow)
    *   [7.2 反序列化和拒绝服务攻击(DoS 攻击)](#deserialization-and-denial-of-service-attack-dos-attack)
*   [8。Java 9 反序列化过滤器](#java-9-deserialization-filters)
    *   [8.1 stack overflow 的反序列化过滤器](#deserialization-filter-for-stackoverflow)
    *   [8.2 针对 DoS 攻击的反序列化过滤器](#deserialization-filter-for-dos-attack)
*   [9。Java 17 特定于上下文的反序列化过滤器](#java-17-context-specific-deserialization-filters)
*   [下载源代码](#download-source-code)
*   [参考文献](#references)

## 1。Hello World Java 序列化

在 Java 中，我们必须实现`Serializable`接口来支持序列化和反序列化。

1.1 一个 Java 对象。

Person.java

```java
 package com.mkyong.io.object;

import java.io.Serializable;
import java.math.BigDecimal;

public class Person implements Serializable {

    private String name;
    private int age;
    private BigDecimal salary;

    // getters setters constructor
} 
```

1.2 Java 对象`Person.java`的完整 Java 序列化和反序列化示例。

HelloSerialization.java

```java
 package com.mkyong.io.object;

import java.io.*;
import java.math.BigDecimal;

public class HelloSerialization {

    public static void main(String[] args) {

        Person person = new Person("mkyong", 40, new BigDecimal(900));

        byte[] bytes = convertObjectToBytes(person);

        Person p = (Person) convertBytesToObject(bytes);

        System.out.println(p);
    }

    // Convert object to byte[]
    public static byte[] convertObjectToBytes(Object obj) {
        ByteArrayOutputStream boas = new ByteArrayOutputStream();
        try (ObjectOutputStream ois = new ObjectOutputStream(boas)) {
            ois.writeObject(obj);
            return boas.toByteArray();
        } catch (IOException ioe) {
            ioe.printStackTrace();
        }
        throw new RuntimeException();
    }

    // Convert byte[] to object
    public static Object convertBytesToObject(byte[] bytes) {
        InputStream is = new ByteArrayInputStream(bytes);
        try (ObjectInputStream ois = new ObjectInputStream(is)) {
            return ois.readObject();
        } catch (IOException | ClassNotFoundException ioe) {
            ioe.printStackTrace();
        }
        throw new RuntimeException();
    }

} 
```

输出

Terminal

```java
 Person{name='mkyong', age=40, salary=900} 
```

## 2。Java . io . notserializableexception

如果我们序列化一个没有实现`Serializable`接口的对象，Java 抛出`java.io.NotSerializableException`。

Person.java

```java
 public class Person {

    private String name;
    private int age;
    private BigDecimal salary;

    // getters setters constructor
} 
```

HelloSerialization.java

```java
 Person person = new Person("mkyong", 40, new BigDecimal(900));

  byte[] bytes = convertObjectToBytes(person); 
```

输出

Terminal

```java
 java.io.NotSerializableException: com.mkyong.io.object.Person
  	at java.base/java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1197)
  	at java.base/java.io.ObjectOutputStream.writeObject(ObjectOutputStream.java:354)
  	at com.mkyong.io.object.HelloSerialization.convertObjectToBytes(HelloSerialization.java:22)
  	at com.mkyong.io.object.HelloSerialization.main(HelloSerialization.java:12)
  Exception in thread "main" java.lang.RuntimeException
  	at com.mkyong.io.object.HelloSerialization.convertObjectToBytes(HelloSerialization.java:27)
  	at com.mkyong.io.object.HelloSerialization.main(HelloSerialization.java:12) 
```

## 3。什么是 serialVersionUID？

如果`serialVersionUID`丢失，JVM 将自动创建它。`serialVersionUID`类似于版本号；简而言之，如果我们用`1L`保存一个对象，我们需要提供相同的`1L`来读取该对象，否则会遇到不兼容的错误。

Person.java

```java
 public class Person implements Serializable {

    private static final long serialVersionUID = 1L;
    //...
} 
```

例如，我们将一个带有`serialVersionUID = 1L`的对象保存到一个文件名`person.obj`中。稍后，我们从对象中添加或删除一些字段，并将`serialVersionUID`更新为`2L`。现在，读取`person.obj`文件，尝试将其转换回修改后的对象；由于两者`serialVersionUID`不同，我们会遇到以下不兼容的错误:

Terminal

```java
 Exception in thread "main" java.io.InvalidClassException: com.mkyong.io.object.Person;

  local class incompatible: stream classdesc serialVersionUID = 1, local class serialVersionUID = 2

	at java.base/java.io.ObjectStreamClass.initNonProxy(ObjectStreamClass.java:689)
	at java.base/java.io.ObjectInputStream.readNonProxyDesc(ObjectInputStream.java:1903)
	at java.base/java.io.ObjectInputStream.readClassDesc(ObjectInputStream.java:1772)
	at java.base/java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:2060)
	at java.base/java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1594)
	at java.base/java.io.ObjectInputStream.readObject(ObjectInputStream.java:430)
	at com.mkyong.io.object.ObjectUtils.readObject(ObjectUtils.java:25)
	at com.mkyong.io.object.ObjectUtils.main(ObjectUtils.java:38) 
```

## 4。什么是瞬变？

在序列化过程中，JVM 忽略所有的`transient`字段。如果我们需要在序列化过程中排除特定的对象字段，将它们标记为`transient.`

Person.java

```java
 public class Person implements Serializable {

    private String name;
    private int age;

    //exclude this field
    private transient BigDecimal salary;

    // getters setters constructor
} 
```

```java
 Person person = new Person("mkyong", 40, new BigDecimal(900));

  byte[] bytes = convertObjectToBytes(person);

  Person p = (Person) convertBytesToObject(bytes);

  System.out.println(p); 
```

输出

Terminal

```java
 Person{name='mkyong', age=40, salary=null} 
```

## 5。将对象序列化到文件

此示例将 Java 对象序列化为文件，并将文件反序列化回原始对象。

HelloSerializationFile.java

```java
 package com.mkyong.io.object;

import java.io.*;
import java.math.BigDecimal;

public class HelloSerializationFile {

  public static void main(String[] args) throws IOException, ClassNotFoundException {

      Person person = new Person("mkyong", 50, new BigDecimal(1000));

      File file = new File("person.anything");

      writeObjectToFile(person, file);

      Person p = readObjectFromFile(file);

      System.out.println(p);

  }

  // Serialization
  // Save object into a file.
  public static void writeObjectToFile(Person obj, File file) throws IOException {
      try (FileOutputStream fos = new FileOutputStream(file);
           ObjectOutputStream oos = new ObjectOutputStream(fos)) {
          oos.writeObject(obj);
          oos.flush();
      }
  }

  // Deserialization
  // Get object from a file.
  public static Person readObjectFromFile(File file) throws IOException, ClassNotFoundException {
      Person result = null;
      try (FileInputStream fis = new FileInputStream(file);
           ObjectInputStream ois = new ObjectInputStream(fis)) {
          result = (Person) ois.readObject();
      }
      return result;
  }

} 
```

## 6。为什么需要 Java 中的序列化？

**注**

阅读这篇文章[Brian Goetz——迈向更好的序列化](http://web.archive.org/web/20220610024935/https://cr.openjdk.java.net/~briangoetz/amber/serialization.html)。

Java 序列化的一些用例:

1.  套接字、应用程序、客户端和服务器之间的数据交换格式。它喜欢 JSON 或 XML，但采用字节格式。
2.  序列化是几项关键 Java 技术的基础，包括 [Java 远程方法调用(Java RMI)](http://web.archive.org/web/20220610024935/https://en.wikipedia.org/wiki/Java_remote_method_invocation) 、公共对象请求代理架构(CORBA)，以及分布式计算，如 Java 命名和目录接口(JNDI)、Java
    管理扩展(JMX)和 Java 消息传递(JMS)。
3.  对于作为轻量级持久性的游戏，我们可以在磁盘上序列化当前游戏的状态，并在以后恢复它。文件是字节格式的，我们不能轻易修改敏感游戏的数据，比如武器或者金钱。
4.  保存一个[对象图](http://web.archive.org/web/20220610024935/https://en.wikipedia.org/wiki/Object_graph)磁盘用于进一步分析，比如 UML 类图。
5.  服务器集群和恢复会话。例如，如果其中一个服务器关闭或重启，服务器集群(服务器 A 和服务器 B)需要同步会话。当服务器 A 关闭时，它将对象保存为`HttpSession`的属性，并通过网络发送给服务器 B，服务器 B 可以从`HttpSession`恢复对象。阅读这篇[的帖子](http://web.archive.org/web/20220610024935/https://stackoverflow.com/questions/2294551/java-io-writeabortedexception-writing-aborted-java-io-notserializableexception)

对于数据交换，考虑人类可读的数据交换格式，如 [JSON](http://web.archive.org/web/20220610024935/https://www.json.org/json-en.html) 、 [XML](http://web.archive.org/web/20220610024935/https://en.wikipedia.org/wiki/XML) 或 [Google 协议缓冲区](http://web.archive.org/web/20220610024935/https://developers.google.com/protocol-buffers)。

## 7。不可信数据的反序列化

但是，从不受信任的字节流进行反序列化是危险的，会导致以下 Java 漏洞:

*   一路向下——以嵌套容器为目标，比如列表中的列表。
*   串行拒绝服务(DOS)
*   河豚
*   远程代码执行(RCE)

**延伸阅读**

*   [邪恶泡菜:基于对象图工程的 DoS 攻击](http://web.archive.org/web/20220610024935/https://homepages.ecs.vuw.ac.nz/~alex/files/DietrichJezekRasheedTahirPotaninECOOP2017.pdf)
*   [OWASP–不可信数据的反序列化](http://web.archive.org/web/20220610024935/https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data)

### 7.1 反序列化和堆栈溢出

反序列化下面的字节流，它抛出`StackOverflowError`。

StackOverflowExample.java

```java
 package com.mkyong.io.object.attack;

import java.io.*;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

public class StackOverflowExample {

    public static void main(String[] args) {

        System.out.println(bomb().length);

        deserialize(bomb());  // throws StackOverflow

        System.out.println("Done");
    }

    static byte[] bomb() {
        HashMap map = new HashMap();
        List list = new ArrayList();
        map.put(list, "");
        list.add(list);
        return serialize(map);
    }

    public static byte[] serialize(Object o) {
        ByteArrayOutputStream ba = new ByteArrayOutputStream();
        try {
            new ObjectOutputStream(ba).writeObject(o);
        } catch (IOException e) {
            throw new IllegalArgumentException(e);
        }
        return ba.toByteArray();
    }

    public static Object deserialize(byte[] bytes) {
        try {
            return new ObjectInputStream(
                    new ByteArrayInputStream(bytes)).readObject();
        } catch (IOException | ClassNotFoundException e) {
            throw new IllegalArgumentException(e);
        }
    }

} 
```

输出

Terminal

```java
 Exception in thread "main" java.lang.StackOverflowError
  at java.base/java.util.ArrayList.hashCode(ArrayList.java:582)
  at java.base/java.util.ArrayList.hashCodeRange(ArrayList.java:595)
  //... 
```

### 7.2 反序列化和拒绝服务攻击(DoS 攻击)

反序列化下面的字节流将保持进程运行，挂起，并慢慢降低系统速度，这是典型的 DoS 攻击。

DosExample.java

```java
 package com.mkyong.io.object.attack;

import java.io.*;
import java.util.HashSet;
import java.util.Set;

public class DosExample {

  public static void main(String[] args) throws Exception {
      System.out.println(bomb().length);

      deserialize(bomb());  // Dos here

      System.out.println("Done");
  }

  static byte[] bomb() {
      Set<Object> root = new HashSet<>();
      Set<Object> s1 = root;
      Set<Object> s2 = new HashSet<>();
      for (int i = 0; i < 100; i++) {
          Set<Object> t1 = new HashSet<>();
          Set<Object> t2 = new HashSet<>();
          t1.add("test-" + i); // make it not equal to t2

          s1.add(t1); // root also add set
          s1.add(t2);

          s2.add(t1);
          s2.add(t2);

          s1 = t1;    // reference to t1, so that `root` can add new set from the last t1
          s2 = t2;
      }
      return serialize(root);
  }

  public static byte[] serialize(Object o) {
      ByteArrayOutputStream ba = new ByteArrayOutputStream();
      try {
          new ObjectOutputStream(ba).writeObject(o);
      } catch (IOException e) {
          throw new IllegalArgumentException(e);
      }
      return ba.toByteArray();
  }

  public static Object deserialize(byte[] bytes) {
      try {
          return new ObjectInputStream(
                  new ByteArrayInputStream(bytes)).readObject();
      } catch (IOException | ClassNotFoundException e) {
          throw new IllegalArgumentException(e);
      }
  }

} 
```

输出

Terminal

```java
 // keep the process running, hanging there
  // and slowly slowing down the system, a classic DoS attack. 
```

## 8。Java 9 反序列化过滤器

Java 9， [JEP 290](http://web.archive.org/web/20220610024935/https://openjdk.java.net/jeps/290) 引入了反序列化过滤器`ObjectInputFilter`来过滤传入的序列化数据。

### 8.1 stack overflow 的反序列化过滤器

用反序列化过滤器`maxdepth=2`重构上面的`StackOverflowExample.java`示例。如果我们重新运行程序，反序列化过程将停止并返回`filter status: REJECTED`。

StackOverflowExample.java

```java
 package com.mkyong.io.object.attack;

import java.io.*;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

public class StackOverflowExample {

  public static void main(String[] args) {
      System.out.println(bomb().length);

      //deserialize(bomb());  // throws StackOverflow

      ObjectInputFilter filter =
              ObjectInputFilter.Config.createFilter(
                      "maxdepth=2;java.base/*;!*");

      // java.io.InvalidClassException: filter status: REJECTED
      deserializeFilter(bomb(), filter);  

      System.out.println("Done");
  }

  static byte[] bomb() {
      HashMap map = new HashMap();
      List list = new ArrayList();
      map.put(list, "");
      list.add(list);
      return serialize(map);
  }

  public static byte[] serialize(Object o) {
      ByteArrayOutputStream ba = new ByteArrayOutputStream();
      try {
          new ObjectOutputStream(ba).writeObject(o);
      } catch (IOException e) {
          throw new IllegalArgumentException(e);
      }
      return ba.toByteArray();
  }

  public static Object deserializeFilter(byte[] bytes, ObjectInputFilter filter) {

      try (ByteArrayInputStream bais = new ByteArrayInputStream(bytes);
           ObjectInputStream ois = new ObjectInputStream(bais)) {

          // add filter before readObject
          ois.setObjectInputFilter(filter);

          return ois.readObject();
      } catch (IOException | ClassNotFoundException e) {
          throw new IllegalArgumentException(e);
      }

  }

} 
```

输出

Terminal

```java
 Exception in thread "main" java.lang.IllegalArgumentException: java.io.InvalidClassException: filter status: REJECTED
  at com.mkyong.io.object.attack.StackOverflowExample.deserializeFilter(StackOverflowExample.java:70)
  at com.mkyong.io.object.attack.StackOverflowExample.main(StackOverflowExample.java:27)
  Caused by: java.io.InvalidClassException: filter status: REJECTED 
```

### 8.2 针对 DoS 攻击的反序列化过滤器

用`maxdepth`反序列化过滤器重构上面的`DosExample.java`示例，以阻止 DoS 攻击。

DosExample.java

```java
 package com.mkyong.io.object.attack;

import java.io.*;
import java.util.HashSet;
import java.util.Set;

public class DosExample {

  public static void main(String[] args) throws Exception {
      System.out.println(bomb().length);

      //deserialize(bomb());  // Dos here

      ObjectInputFilter filter =
              ObjectInputFilter.Config.createFilter(
                      "maxdepth=10;java.base/*;!*");

      deserializeFilter(bomb(), filter);  // Dos here

      System.out.println("Done");
  }

  static byte[] bomb() {
      Set<Object> root = new HashSet<>();
      Set<Object> s1 = root;
      Set<Object> s2 = new HashSet<>();
      for (int i = 0; i < 100; i++) {
          Set<Object> t1 = new HashSet<>();
          Set<Object> t2 = new HashSet<>();
          t1.add("test-" + i); // make it not equal to t2

          s1.add(t1); // root also add set
          s1.add(t2);

          s2.add(t1);
          s2.add(t2);

          s1 = t1;    // reference to t1, so that `root` can add new set from the last t1
          s2 = t2;
      }
      return serialize(root);
  }

  public static byte[] serialize(Object o) {
      ByteArrayOutputStream ba = new ByteArrayOutputStream();
      try {
          new ObjectOutputStream(ba).writeObject(o);
      } catch (IOException e) {
          throw new IllegalArgumentException(e);
      }
      return ba.toByteArray();
  }

  public static Object deserializeFilter(byte[] bytes, ObjectInputFilter filter) {

      try (ByteArrayInputStream bais = new ByteArrayInputStream(bytes);
           ObjectInputStream ois = new ObjectInputStream(bais)) {

          // add filter before readObject
          ois.setObjectInputFilter(filter);

          return ois.readObject();
      } catch (IOException | ClassNotFoundException e) {
          throw new IllegalArgumentException(e);
      }

  }

} 
```

如果我们重新运行程序，反序列化过程将停止并返回`filter status: REJECTED`。

Terminal

```java
 Exception in thread "main" java.lang.IllegalArgumentException:
  java.io.InvalidClassException: filter status: REJECTED
  at com.mkyong.io.object.attack.DosExample.deserializeFilter(DosExample.java:74)
  at com.mkyong.io.object.attack.DosExample.main(DosExample.java:19) 
```

**注**

阅读关于[序列化过滤](http://web.archive.org/web/20220610024935/https://docs.oracle.com/en/java/javase/17/core/serialization-filtering1.html)的官方指南。

## 9。Java 17 特定于上下文的反序列化过滤器

Java 17 [JEP 415](http://web.archive.org/web/20220610024935/https://openjdk.java.net/jeps/415) 引入了一个过滤器工厂，它允许动态或上下文相关地选择不同的反序列化过滤器，参考 [Java 17 过滤器工厂示例](http://web.archive.org/web/20220610024935/https://mkyong.com/java/what-is-new-in-java-17/#jep-415-context-specific-deserialization-filters)。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java.git](http://web.archive.org/web/20220610024935/https://github.com/mkyong/core-java.git)

$ CD Java-io/com/mkyong/io/object

## 参考文献

*   [Java 版本历史](http://web.archive.org/web/20220610024935/https://en.wikipedia.org/wiki/Java_version_history)
*   [序列化过滤](http://web.archive.org/web/20220610024935/https://docs.oracle.com/en/java/javase/17/core/serialization-filtering1.html)
*   [OWASP–不可信数据的反序列化](http://web.archive.org/web/20220610024935/https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data)
*   [Brian Goetz——迈向更好的序列化](http://web.archive.org/web/20220610024935/https://cr.openjdk.java.net/~briangoetz/amber/serialization.html)
*   [邪恶泡菜:基于对象图工程的 DoS 攻击](http://web.archive.org/web/20220610024935/https://homepages.ecs.vuw.ac.nz/~alex/files/DietrichJezekRasheedTahirPotaninECOOP2017.pdf)
*   [Java 对象序列化规范](http://web.archive.org/web/20220610024935/https://docs.oracle.com/en/java/javase/11/docs/specs/serialization/input.html)
*   [可序列化的 JavaDoc](http://web.archive.org/web/20220610024935/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/Serializable.html)
*   [ObjectInputStream JavaDoc](http://web.archive.org/web/20220610024935/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/ObjectInputStream.html)
*   [ObjectOutputStreamJavaDoc](http://web.archive.org/web/20220610024935/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/ObjectOutputStream.html)
*   [Java–什么是 serialVersionUID](/web/20220610024935/https://mkyong.com/java-best-practices/understand-the-serialversionuid/)
*   [JEP 290:过滤输入的串行化数据](http://web.archive.org/web/20220610024935/https://openjdk.java.net/jeps/290)
*   WebLogic、WebSphere、JBoss、Jenkins、OpenNMS 和您的应用程序有什么共同点？这个漏洞。
*   [Java 序列化漏洞](http://web.archive.org/web/20220610024935/https://www.contrastsecurity.com/security-influencers/protect-your-apps-from-java-serialization-vulnerability)

<input type="hidden" id="mkyong-current-postId" value="15705">