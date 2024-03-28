# 声明注解类型

> 原文：[`docs.oracle.com/javase/tutorial/java/annotations/declaring.html`](https://docs.oracle.com/javase/tutorial/java/annotations/declaring.html)

许多注解取代了代码中的注释。

假设一个软件组传统上在每个类的主体部分以提供重要信息的注释开头：

```java
public class Generation3List extends Generation2List {

   // Author: John Doe
   // Date: 3/17/2002
   // Current revision: 6
   // Last modified: 4/12/2004
   // By: Jane Doe
   // Reviewers: Alice, Bill, Cindy

   // class code goes here

}

```

要使用注解添加相同的元数据，必须首先定义*注解类型*。这样做的语法是：

```java
@interface ClassPreamble {
   String author();
   String date();
   int currentRevision() default 1;
   String lastModified() default "N/A";
   String lastModifiedBy() default "N/A";
   // Note use of array
   String[] reviewers();
}

```

注解类型定义看起来类似于接口定义，其中关键字`interface`之前有一个 at 符号(`@`)（@ = AT，表示注解类型）。注解类型是*接口*的一种形式，稍后会介绍。目前，您不需要理解接口。

前一个注解定义的主体包含*注解类型元素*声明，看起来很像方法。请注意，它们可以定义可选的默认值。

在定义注解类型之后，您可以像这样使用该类型的注解，填入值：

```java
@ClassPreamble (
   author = "John Doe",
   date = "3/17/2002",
   currentRevision = 6,
   lastModified = "4/12/2004",
   lastModifiedBy = "Jane Doe",
   // Note array notation
   reviewers = {"Alice", "Bob", "Cindy"}
)
public class Generation3List extends Generation2List {

// class code goes here

}

```

* * *

**注意：** 要使`@ClassPreamble`中的信息出现在 Javadoc 生成的文档中，必须使用`@Documented`注解注释`@ClassPreamble`定义：

```java
// import this to use @Documented
import java.lang.annotation.*;

@Documented
@interface ClassPreamble {

   // Annotation element definitions

}

```

* * *
