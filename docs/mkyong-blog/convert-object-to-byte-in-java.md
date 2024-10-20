# 在 Java 中将对象转换为 byte[]

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/convert-object-to-byte-in-java/>

这篇文章展示了如何在 Java 中将一个对象转换成`byte[]`或字节数组，反之亦然。

## 1。将对象转换为字节[]

下面的例子展示了如何使用`ByteArrayOutputStream`和`ObjectOutputStream`将一个对象转换为`byte[]`。

```java
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

  // Convert object to byte[]
  public static byte[] convertObjectToBytes2(Object obj) throws IOException {
      ByteArrayOutputStream boas = new ByteArrayOutputStream();
      try (ObjectOutputStream ois = new ObjectOutputStream(boas)) {
          ois.writeObject(obj);
          return boas.toByteArray();
      }
  } 
```

## 2。将字节[]转换为对象

下面的例子展示了如何使用`ByteArrayInputStream`和`ObjectInputStream`将`byte[]`转换回一个对象。

```java
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

  // Convert byte[] to object
  public static Object convertBytesToObject2(byte[] bytes)
      throws IOException, ClassNotFoundException {
      InputStream is = new ByteArrayInputStream(bytes);
      try (ObjectInputStream ois = new ObjectInputStream(is)) {
          return ois.readObject();
      }
  }

  // Convert byte[] to object with filter
  public static Object convertBytesToObjectWithFilter(byte[] bytes, ObjectInputFilter filter) {
      InputStream is = new ByteArrayInputStream(bytes);
      try (ObjectInputStream ois = new ObjectInputStream(is)) {

          // add filter before readObject
          ois.setObjectInputFilter(filter);

          return ois.readObject();
      } catch (IOException | ClassNotFoundException ioe) {
          ioe.printStackTrace();
      }
      throw new RuntimeException();
  } 
```

**延伸阅读**

*   [Java 序列化和反序列化示例](http://web.archive.org/web/20221230032550/https://mkyong.com/java/java-serialization-examples/)

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java.git](http://web.archive.org/web/20221230032550/https://github.com/mkyong/core-java.git)

$ CD Java-io/com/mkyong/io/object

## 参考文献

*   [文件到字节[]](http://web.archive.org/web/20221230032550/https://mkyong.com/java/how-to-convert-file-into-an-array-of-bytes/)
*   [JavaDoc 对象输入流](http://web.archive.org/web/20221230032550/https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/ObjectInputStream.html)
*   [JavaDoc object output stream](http://web.archive.org/web/20221230032550/https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/ObjectOutputStream.html)
*   [JavaDoc ByteArrayOutputStream](http://web.archive.org/web/20221230032550/https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/ByteArrayOutputStream.html)
*   [JavaDoc ByteArrayInputStream](http://web.archive.org/web/20221230032550/https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/ByteArrayInputStream.html)
*   [OWASP–不可信数据的反序列化](http://web.archive.org/web/20221230032550/https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data)
*   [Brian Goetz——迈向更好的序列化](http://web.archive.org/web/20221230032550/https://cr.openjdk.java.net/~briangoetz/amber/serialization.html)

<input type="hidden" id="mkyong-current-postId" value="17028">