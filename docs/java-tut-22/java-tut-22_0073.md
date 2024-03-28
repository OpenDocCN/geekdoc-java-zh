# 注解基础知识

> 原文：[`docs.oracle.com/javase/tutorial/java/annotations/basics.html`](https://docs.oracle.com/javase/tutorial/java/annotations/basics.html)

## 注解的格式

在其最简单的形式下，注解看起来像下面这样：

```java
@Entity

```

符号`@`告诉编译器接下来是一个注解。在下面的例子中，注解的名称是`Override`：

```java
@Override
void mySuperMethod() { ... }

```

注解可以包括*元素*，这些元素可以是命名的或未命名的，并且这些元素有值：

```java
@Author(
   name = "Benjamin Franklin",
   date = "3/27/2003"
)
class MyClass { ... }

```

或

```java
@SuppressWarnings(value = "unchecked")
void myMethod() { ... }

```

如果只有一个名为`value`的元素，则名称可以省略，如：

```java
@SuppressWarnings("unchecked")
void myMethod() { ... }

```

如果注解没有元素，则括号可以省略，如前面的`@Override`示例所示。

也可以在同一声明上使用多个注解：

```java
@Author(name = "Jane Doe")
@EBook
class MyClass { ... }

```

如果注解具有相同的类型，则称为重复注解：

```java
@Author(name = "Jane Doe")
@Author(name = "John Smith")
class MyClass { ... }

```

从 Java SE 8 发布开始支持重复注解。更多信息，请参见重复注解。

注解类型可以是 Java SE API 的`java.lang`或`java.lang.annotation`包中定义的类型之一。在前面的示例中，`Override`和`SuppressWarnings`是预定义的 Java 注解。还可以定义自己的注解类型。前面示例中的`Author`和`Ebook`注解是自定义注解类型。

## 注解可以使用的位置

注解可以应用于声明：类、字段、方法和其他程序元素的声明。当用于声明时，每个注解通常按照惯例出现在自己的一行上。

从 Java SE 8 发布开始，注解也可以应用于类型的*使用*。以下是一些示例：

+   类实例创建表达式：

    ```java
        new @Interned MyObject();

    ```

+   类型转换：

    ```java
        myString = (@NonNull String) str;

    ```

+   `implements`子句：

    ```java
        class UnmodifiableList<T> implements
            @Readonly List<@Readonly T> { ... }

    ```

+   抛出异常声明：

    ```java
        void monitorTemperature() throws
            @Critical TemperatureException { ... }

    ```

这种形式的注解称为*类型注解*。更多信息，请参见类型注解和可插入类型系统。
