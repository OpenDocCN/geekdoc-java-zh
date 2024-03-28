# 长期持久性

> 原文：[`docs.oracle.com/javase/tutorial/javabeans/advanced/longpersistence.html`](https://docs.oracle.com/javase/tutorial/javabeans/advanced/longpersistence.html)

*长期持久性*是一种模型，可以将 bean 保存为 XML 格式。

有关 XML 格式和如何为非 bean 实现长期持久性的信息，请参阅[XML 模式](http://www.oracle.com/technetwork/java/persistence3-139471.html)和[使用 XMLEncoder](http://www.oracle.com/technetwork/java/persistence4-140124.html)。

## 编码器和解码器

[`XMLEncoder`](https://docs.oracle.com/javase/8/docs/api/java/beans/XMLEncoder.html)类用于编写`Serializable`对象的文本表示的输出文件。以下代码片段是将 Java bean 及其属性以 XML 格式写入的示例：

```java
XMLEncoder encoder = new XMLEncoder(
           new BufferedOutputStream(
           new FileOutputStream("Beanarchive.xml")));

encoder.writeObject(object);
encoder.close(); 

```

[`XMLDecoder`](https://docs.oracle.com/javase/8/docs/api/java/beans/XMLDecoder.html)类读取使用 XMLEncoder 创建的 XML 文档：

```java
XMLDecoder decoder = new XMLDecoder(
    new BufferedInputStream(
    new FileInputStream("Beanarchive.xml")));

Object object = decoder.readObject();
decoder.close();

```

## XML 中有什么？

XML bean 存档具有自己特定的语法，其中包括以下标签来表示每个 bean 元素：

+   用于描述 XML 版本和编码类型的 XML 前言

+   一个`**<java>**`标签，用于包含 bean 的所有对象元素

+   一个`**<object>**`标签，用于表示从其序列化形式重建对象所需的一组方法调用

    ```java
    <object class="javax.swing.JButton" method="new">
        <string>Ok</string>
    </object>

    ```

    或语句

    ```java
    <object class="javax.swing.JButton">
        <void method="setText">
            <string>Cancel</string>
                </void>
        </object>

    ```

+   用于定义适当的基本类型的标签：

    +   `**<boolean>**`

    +   `**<byte>**`

    +   `**<char>**`

    +   `**<short>**`

    +   `**<int>**`

    +   `**<long>**`

    +   `**<float>**`

    +   `**<double>**`

    ```java
    <int>5555</int>

    ```

+   一个**<`class`>**标签，用于表示 Class 的一个实例。

    ```java
    <class>java.swing.JFrame</class>

    ```

+   一个**<`array`>**标签用于定义数组

    ```java
    <array class="java.lang.String" length="5">
    </array>

    ```

以下代码表示将为`SimpleBean`组件生成的 XML 存档：

```java
<?xml version="1.0" encoding="UTF-8" ?>
<java>
  <object class="javax.swing.JFrame">
    <void method="add">
      <object class="java.awt.BorderLayout" field="CENTER"/>
      <object class="SimpleBean"/>
    </void>
    <void property="defaultCloseOperation">
      <object class="javax.swing.WindowConstants" field="DISPOSE_ON_CLOSE"/>
    </void>
    <void method="pack"/>
    <void property="visible">
      <boolean>true</boolean>
    </void>
  </object>
</java>

```
