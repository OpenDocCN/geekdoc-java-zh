# 可重复注解

> 原文：[`docs.oracle.com/javase/tutorial/java/annotations/repeating.html`](https://docs.oracle.com/javase/tutorial/java/annotations/repeating.html)

有一些情况下，您希望将相同的注解应用于声明或类型使用。从 Java SE 8 发布开始，*可重复注解*使您能够做到这一点。

例如，您正在编写代码以使用一个定时器服务，该服务使您能够在指定时间运行一个方法或按照某个计划运行，类似于 UNIX 的`cron`服务。现在您想设置一个定时器在每个月的最后一天和每个星期五晚上 11 点运行一个`doPeriodicCleanup`方法。要设置定时器运行，创建一个`@Schedule`注解并将其应用两次于`doPeriodicCleanup`方法。第一次使用指定了每月的最后一天，第二次指定了星期五晚上 11 点，如下面的代码示例所示：

```java
@Schedule(dayOfMonth="last")
@Schedule(dayOfWeek="Fri", hour="23")
public void doPeriodicCleanup() { ... }

```

前面的示例对一个方法应用了一个注解。您可以在任何需要使用标准注解的地方重复使用注解。例如，您有一个处理未经授权访问异常的类。您为经理们注解了一个`@Alert`注解，为管理员注解了另一个：

```java
@Alert(role="Manager")
@Alert(role="Administrator")
public class UnauthorizedAccessException extends SecurityException { ... }

```

由于兼容性原因，重复注解存储在 Java 编译器自动生成的*容器注解*中。为了让编译器做到这一点，您的代码中需要两个声明。

## 第 1 步：声明一个可重复的注解类型

注解类型必须标记为`@Repeatable`元注解。以下示例定义了一个自定义的`@Schedule`可重复注解类型：

```java
import java.lang.annotation.Repeatable;

@Repeatable(Schedules.class)
public @interface Schedule {
  String dayOfMonth() default "first";
  String dayOfWeek() default "Mon";
  int hour() default 12;
}

```

`@Repeatable`元注解的值（括号中）是 Java 编译器生成的用于存储重复注解的容器注解的类型。在本例中，包含注解类型是`Schedules`，因此重复的`@Schedule`注解存储在一个`@Schedules`注解中。

在未声明为可重复之前将相同的注解应用于声明会导致编译时错误。

## 第 2 步：声明包含注解类型

包含注解类型必须具有一个带有数组类型的`value`元素。数组类型的组件类型必须是可重复注解类型。`Schedules`包含注解类型的声明如下：

```java
public @interface Schedules {
    Schedule[] value();
}

```

## 检索注解

反射 API 中有几种可用的方法可用于检索注解。返回单个注解的方法的行为，例如[AnnotatedElement.getAnnotation(Class<T>)](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html#getAnnotation-java.lang.Class-)，在只有*一个*请求类型的注解存在时保持不变。如果存在多个请求类型的注解，可以通过首先获取它们的容器注解来获取它们。通过这种方式，旧代码仍然可以正常工作。在 Java SE 8 中引入了其他方法，通过扫描容器注解一次返回多个注解，例如[AnnotatedElement.getAnnotationsByType(Class<T>)](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html#getAnnotationsByType-java.lang.Class-)。请参阅[AnnotatedElement](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html)类规范，了解所有可用方法的信息。

## 设计考虑事项

当设计注解类型时，您必须考虑该类型的注解的*基数*。现在可以使用一个注解零次，一次，或者，如果注解的类型标记为`@Repeatable`，可以使用多次。还可以通过使用`@Target`元注解来限制注解类型可以在哪里使用。例如，您可以创建一个可重复使用的注解类型，只能用于方法和字段。设计注解类型时要仔细考虑，以确保使用注解的程序员发现它尽可能灵活和强大。
