# 理解默认清单

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/defman.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/defman.html)

当您创建一个 JAR 文件时，它会自动获得一个默认清单文件。存档中只能有一个清单文件，并且它始终具有路径名

```java
META-INF/MANIFEST.MF

```

当您创建一个 JAR 文件时，默认清单文件只包含以下内容：

```java
Manifest-Version: 1.0
Created-By: 1.7.0_06 (Oracle Corporation)

```

这些行显示清单条目采用“头部: 值”对的形式。头部的名称与其值之间用冒号分隔。默认清单符合清单规范的 1.0 版本，并由 JDK 的 1.7.0_06 版本创建。

清单还可以包含有关打包在存档中的其他文件的信息。清单中应记录哪些文件信息取决于您打算如何使用 JAR 文件。默认清单不假设应记录有关其他文件的哪些信息。

信息摘要不包含在默认清单中。要了解有关摘要和签名的更多信息，请参阅签署和验证 JAR 文件课程。
