# 下载并尝试示例应用程序

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/step1.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/step1.html)

本课程使用我们提供给您的一个简单应用程序。

1.  通过复制或下载`Count.java`源代码，在本地计算机上创建一个名为`Count.java`的文件。本课程中的示例假定您将`count`放在`C:\Test`目录中。

1.  `count`应用程序需要访问包含其将处理的数据的文本文件。下载一个`示例数据文件`，或使用任何其他方便的文本文件作为数据。

    * * *

    **重要：**将数据文件放入一个**不同于**包含已下载`count`类文件的目录中。我们建议使用`C:\TestData\data`。

    在本课程的后面部分，您将看到在安全管理器下运行的应用程序除非有明确的权限，否则无法读取文件。但是，应用程序*始终*可以从相同目录（或子目录）中读取文件。它不需要明确的权限。

    * * *

1.  编译然后运行`Count`应用程序以查看其功能。

    当您运行`count`应用程序时，您需要指定（作为参数）要读取的文件的路径名。

    `java Count C:\TestData\data`

这里是一个示例运行：

```java
    C:\Test>java Count C:\TestData\data
    Counted 65 chars.

```
