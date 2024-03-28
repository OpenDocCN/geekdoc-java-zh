# 设置包版本信息

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/packageman.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/packageman.html)

您可能需要在 JAR 文件的 MANIFEST.MF 中包含包版本信息。您可以使用 MANIFEST.MF 中的以下头部提供此信息：

**清单中的头部**

| 头部 | 定义 |
| --- | --- |
| `Name` | 规范的名称。 |
| `Specification-Title` | 规范的标题。 |
| `Specification-Version` | 规范的版本。 |
| `Specification-Vendor` | 规范的供应商。 |
| `Implementation-Title` | 实现的标题。 |
| `Implementation-Version` | 实现的构建编号。 |
| `Implementation-Vendor` | 实现的供应商。 |

每个包可以分配一组这样的头部。版本头部应直接出现在包的`Name`头部下方。此示例显示了所有版本头部：

```java
Name: java/util/
Specification-Title: Java Utility Classes
Specification-Version: 1.2
Specification-Vendor: Example Tech, Inc.
Implementation-Title: java.util
Implementation-Version: build57
Implementation-Vendor: Example Tech, Inc.

```

关于包版本头的更多信息，请参阅[包版本规范](https://docs.oracle.com/javase/8/docs/technotes/guides/versioning/spec/versioning2.html#wp89936)。

## 一个示例

我们希望在 MyJar.jar 的 MANIFEST.MF 中包含上述示例中的头部。

首先创建一个名为`Manifest.txt`的文本文件，内容如下：

```java
Name: java/util/
Specification-Title: Java Utility Classes
Specification-Version: 1.2
Specification-Vendor: Example Tech, Inc.
Implementation-Title: java.util 
Implementation-Version: build57
Implementation-Vendor: Example Tech, Inc.

```

* * *

**警告：** 文本文件必须以新行或回车符结束。如果最后一行没有以新行或回车符结束，将无法正确解析最后一行。

* * *

然后通过输入以下命令创建名为`MyJar.jar`的 JAR 文件：

```java
jar cfm MyJar.jar Manifest.txt MyPackage/*.class

```

这将创建带有以下内容的 MANIFEST.MF 文件的 JAR 文件：

```java
Manifest-Version: 1.0
Created-By: 1.7.0_06 (Oracle Corporation)
Name: java/util/
Specification-Title: Java Utility Classes
Specification-Version: 1.2
Specification-Vendor: Example Tech, Inc.
Implementation-Title: java.util 
Implementation-Version: build57
Implementation-Vendor: Example Tech, Inc.

```
