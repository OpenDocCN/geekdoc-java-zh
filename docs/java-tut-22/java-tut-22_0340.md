# 在 JAR 文件中封装包

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/sealman.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/sealman.html)

JAR 文件中的包可以选择性地封装，这意味着该包中定义的所有类必须存档在同一个 JAR 文件中。例如，您可能希望封装一个包，以确保软件中的类之间的版本一致性。

通过在 MANIFEST.MF 中添加`Sealed`头部来封装 JAR 文件中的包，其一般形式为：

```java
Name: myCompany/myPackage/
Sealed: true

```

值`myCompany/myPackage/`是要封装的包的名称。

请注意，包名必须以“/”结尾。

## 一个示例

我们想要在 JAR 文件`MyJar.jar`中封装两个包`firstPackage`和`secondPackage`。

首先，我们创建一个名为`Manifest.txt`的文本文件，内容如下：

```java
Name: myCompany/firstPackage/
Sealed: true

Name: myCompany/secondPackage/
Sealed: true

```

* * *

**警告：** 文本文件必须以新行或回车符结束。如果最后一行没有以新行或回车符结束，将无法正确解析最后一行。

* * *

然后，我们通过输入以下命令创建一个名为`MyJar.jar`的 JAR 文件：

```java
jar cfm MyJar.jar Manifest.txt MyPackage/*.class

```

这将创建一个带有以下内容的 MANIFEST.MF 文件的 JAR 文件：

```java
Manifest-Version: 1.0
Created-By: 1.7.0_06 (Oracle Corporation)
Name: myCompany/firstPackage/
Sealed: true
Name: myCompany/secondPackage/
Sealed: true

```

## 封装 JAR 文件

如果要确保包中的所有类来自同一代码源，请使用 JAR 封装。封装的 JAR 指定该 JAR 定义的所有包都是封装的，除非在每个包的基础上进行覆盖。

要封装一个 JAR 文件，请使用值为 true 的`Sealed` MANIFEST.MF 头部。例如，

```java
Sealed: true

```

指定此存档中的所有包都是封装的，除非在 MANIFEST.MF 条目中使用`Sealed`属性明确覆盖特定包。
