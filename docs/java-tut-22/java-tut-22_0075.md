# 预定义的注解类型

> 原文：[`docs.oracle.com/javase/tutorial/java/annotations/predefined.html`](https://docs.oracle.com/javase/tutorial/java/annotations/predefined.html)

一组注解类型在 Java SE API 中预定义。一些注解类型由 Java 编译器使用，而一些适用于其他注解。

## Java 语言使用的注解类型

在`java.lang`中定义的预定义注解类型为`@Deprecated`、`@Override`和`@SuppressWarnings`。

**@Deprecated** [`@Deprecated`](https://docs.oracle.com/javase/8/docs/api/java/lang/Deprecated.html) 注解表示标记的元素已被*弃用*，不应再使用。每当程序使用带有`@Deprecated`注解的方法、类或字段时，编译器都会生成警告。当元素被弃用时，还应使用 Javadoc 的`@deprecated`标签进行文档化，如下例所示。在 Javadoc 注释和注解中使用`@`符号并非巧合：它们在概念上是相关的。另外，请注意，Javadoc 标签以小写*d*开头，而注解以大写*D*开头。

```java
   // Javadoc comment follows
    /**
     * *@deprecated*
     * *explanation of why it was deprecated*
     */
    @Deprecated
    static void deprecatedMethod() { }
}

```

**@Override** [`@Override`](https://docs.oracle.com/javase/8/docs/api/java/lang/Override.html) 注解告诉编译器，该元素意在覆盖在超类中声明的元素。覆盖方法将在接口和继承中讨论。

```java
   // *mark method as a superclass method*
   // *that has been overridden*
   @Override 
   int overriddenMethod() { }

```

虽然在覆盖方法时不是必须使用此注解，但它有助于防止错误。如果标记为`@Override`的方法未能正确覆盖其超类中的方法，编译器将生成错误。

**@SuppressWarnings** [`@SuppressWarnings`](https://docs.oracle.com/javase/8/docs/api/java/lang/SuppressWarnings.html) 注解告诉编译器抑制其通常生成的特定警告。在下面的示例中，使用了一个已弃用的方法，编译器通常会生成警告。然而，在这种情况下，该注解导致警告被抑制。

```java
   // *use a deprecated method and tell* 
   // *compiler not to generate a warning*
   @SuppressWarnings("deprecation")
    void useDeprecatedMethod() {
        // deprecation warning
        // - suppressed
        objectOne.deprecatedMethod();
    }

```

每个编译器警告都属于一个类别。Java 语言规范列出了两个类别：`deprecation`和`unchecked`。当与在泛型出现之前编写的旧代码进行接口时，可能会出现`unchecked`警告。要抑制多个类别的警告，请使用以下语法：

```java
@SuppressWarnings({"unchecked", "deprecation"})

```

**@SafeVarargs** [`@SafeVarargs`](https://docs.oracle.com/javase/8/docs/api/java/lang/SafeVarargs.html) 注解，当应用于方法或构造函数时，断言代码不对其`varargs`参数执行潜在不安全的操作。使用此注解类型时，与`varargs`使用相关的未经检查的警告将被抑制。

**@FunctionalInterface** [`@FunctionalInterface`](https://docs.oracle.com/javase/8/docs/api/java/lang/FunctionalInterface.html) 注解，引入于 Java SE 8，指示类型声明旨在成为功能接口，如 Java 语言规范所定义。

## 适用于其他注解的注解

适用于其他注解的注解称为*元注解*。在`java.lang.annotation`中定义了几种元注解类型。

**@Retention** [`@Retention`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Retention.html) 注解指定标记的注解如何存储：

+   `RetentionPolicy.SOURCE` – 标记的注解仅在源级别保留，并被编译器忽略。

+   `RetentionPolicy.CLASS` – 标记的注解在编译时由编译器保留，但在 Java 虚拟机（JVM）中被忽略。

+   `RetentionPolicy.RUNTIME` – 标记的注解由 JVM 保留，因此可以被运行时环境使用。

**@Documented** [`@Documented`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Documented.html) 注解指示每当使用指定的注解时，应使用 Javadoc 工具记录这些元素。（默认情况下，注解不包含在 Javadoc 中。）有关更多信息，请参阅[Javadoc 工具页面](https://docs.oracle.com/javase/8/docs/technotes/guides/javadoc/index.html)。

**@Target** [`@Target`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Target.html) 注解标记另一个注解，限制注解可以应用于哪种 Java 元素。目标注解将以下元素类型之一指定为其值：

+   `ElementType.ANNOTATION_TYPE` 可以应用于注解类型。

+   `ElementType.CONSTRUCTOR` 可以应用于构造函数。

+   `ElementType.FIELD` 可以应用于字段或属性。

+   `ElementType.LOCAL_VARIABLE` 可以应用于局部变量。

+   `ElementType.METHOD` 可以应用于方法级别的注解。

+   `ElementType.PACKAGE` 可以应用于包声明。

+   `ElementType.PARAMETER` 可以应用于方法的参数。

+   `ElementType.TYPE` 可以应用于类的任何元素。

**@Inherited** [`@Inherited`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Inherited.html) 注解指示注解类型可以从超类继承。（默认情况下不是这样。）当用户查询注解类型并且类没有此类型的注解时，将查询类的超类以获取注解类型。此注解仅适用于类声明。

**@Repeatable** [`@Repeatable`](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Repeatable.html) 注解，引入于 Java SE 8，表示标记的注解可以多次应用于同一声明或类型使用。有关更多信息，请参阅重复注解。
