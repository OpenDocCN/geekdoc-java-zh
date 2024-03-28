# 管理源文件和类文件

> 原文：[`docs.oracle.com/javase/tutorial/java/package/managingfiles.html`](https://docs.oracle.com/javase/tutorial/java/package/managingfiles.html)

Java 平台的许多实现依赖于分层文件系统来管理源文件和类文件，尽管*Java 语言规范*并不要求这样做。策略如下。

将一个类、接口、枚举或注解类型的源代码放在一个文本文件中，文件名为类型的简单名称，扩展名为`.java`。例如：

```java
//in the Rectangle.java file 
package graphics;
public class Rectangle {
   ... 
}

```

然后，将源文件放在一个反映类型所属包名的目录中：

```java
.....\graphics\Rectangle.java

```

包成员的限定名称和文件的路径名称是平行的，假设使用 Microsoft Windows 文件名分隔符反斜杠（对于 UNIX，请使用正斜杠）。

+   **类名** – `graphics.Rectangle`

+   **文件路径** – `graphics\Rectangle.java`

正如你应该记得的那样，按照惯例，公司使用其反转的互联网域名作为其包名。例如，其互联网域名为`example.com`的 Example 公司将在其所有包名之前加上`com.example`。包名的每个组件对应一个子目录。因此，如果 Example 公司有一个包含`Rectangle.java`源文件的`com.example.graphics`包，它将包含在一系列子目录中，如下所示：

```java
....\com\example\graphics\Rectangle.java

```

当你编译一个源文件时，编译器为其中定义的每个类型创建一个不同的输出文件。输出文件的基本名称是类型的名称，其扩展名是`.class`。例如，如果源文件如下所示

```java
//in the Rectangle.java file
package com.example.graphics;
public class Rectangle {
      . . . 
}

class Helper{
      . . . 
}

```

然后编译后的文件将位于：

```java
<path to the parent directory of the output files>\com\example\graphics\Rectangle.class
<path to the parent directory of the output files>\com\example\graphics\Helper.class

```

与`.java`源文件一样，编译后的`.class`文件应该在反映包名的一系列目录中。然而，`.class`文件的路径不一定要与`.java`源文件的路径相同。你可以将源文件和类文件目录分开管理，如：

```java
<path_one>\sources\com\example\graphics\Rectangle.java

<path_two>\classes\com\example\graphics\Rectangle.class

```

通过这样做，你可以将`classes`目录提供给其他程序员，而不会泄露你的源代码。你还需要以这种方式管理源文件和类文件，以便编译器和 Java 虚拟机（JVM）可以找到程序中使用的所有类型。

`classes`目录的完整路径，`<path_two>\classes`，被称为*类路径*，并通过`CLASSPATH`系统变量设置。编译器和 JVM 都会通过将包名添加到类路径来构建到你的`.class`文件的路径。例如，如果

```java
<path_two>\classes

```

是你的类路径，包名是

```java
com.example.graphics,

```

然后编译器和 JVM 会在以下位置查找`.class`文件

```java
<path_two>\classes\com\example\graphics.

```

类路径可以包括多个路径，用分号（Windows）或冒号（UNIX）分隔。默认情况下，编译器和 JVM 会搜索当前目录和包含 Java 平台类的 JAR 文件，因此这些目录会自动包含在你的类路径中。

## 设置 CLASSPATH 系统变量

要在 Windows 和 UNIX（Bourne shell）中显示当前的`CLASSPATH`变量，请使用以下命令：

```java
In Windows:   C:\> set CLASSPATH
In UNIX:      % echo $CLASSPATH

```

要删除当前`CLASSPATH`变量的内容，请使用以下命令：

```java
In Windows:   C:\> set CLASSPATH=
In UNIX:      % unset CLASSPATH; export CLASSPATH

```

要设置`CLASSPATH`变量，请使用以下命令（例如）：

```java
In Windows:   C:\> set CLASSPATH=C:\users\george\java\classes
In UNIX:      % CLASSPATH=/home/george/java/classes; export CLASSPATH

```
