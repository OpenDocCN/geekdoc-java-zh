# 查看策略文件效果

> 原文：[`docs.oracle.com/javase/tutorial/security/toolsign/rstep4.html`](https://docs.oracle.com/javase/tutorial/security/toolsign/rstep4.html)

在之前的步骤中，你在`exampleraypolicy`策略文件中创建了一个条目，授予由`susan`签名的代码从`C:\TestData\`目录（或者如果你在 UNIX 上工作，则是你的主目录中的`testdata`目录）读取文件的权限。现在，你应该能够成功地执行`Count`程序，从指定目录中读取文件并计算字符数，即使在使用安全管理器运行应用程序时也是如此。

如在创建策略文件课程的结尾所述，有两种可能的方式可以使`exampleraypolicy`文件作为整体策略的一部分被考虑，除了在安全属性文件中指定的策略文件之外。第一种方法是在传递给运行时系统的属性中指定额外的策略文件。第二种方法是在安全属性文件中添加一行指定额外的策略文件。

## 方法 1

你可以使用`-Djava.security.policy`命令行参数来指定一个策略文件，该文件应该被用来补充或替代安全属性文件中指定的文件。

要运行`Count`应用程序并包含`exampleraypolicy`策略文件，请在包含`sCount.jar`和`exampleraypolicy`文件的目录中键入以下内容：

```java
java -Djava.security.manager
    -Djava.security.policy=exampleraypolicy
    -cp sCount.jar Count C:\TestData\data

```

注意：在一行上键入命令，`-D`和`-cp`之前加上一个空格。

程序应该报告指定文件中的字符数。

如果仍然报错，那么策略文件中可能存在问题。使用策略工具检查你在上一步中创建的权限，并更正任何拼写错误或其他错误。

## 方法 2

你可以在安全属性文件中的`policy.url.n`属性中指定多个 URL，包括形如"http://"的 URL，所有指定的策略文件都将被加载。

因此，让解释器考虑你的`exampleraypolicy`文件的策略条目的一种方法是在安全属性文件中添加指示该文件的条目。

* * *

**重要提示：**如果你正在运行自己的 JDK 副本，你可以轻松编辑你的安全属性文件。如果你正在运行与他人共享的版本，只有在你有写入权限或在适当时向系统管理员请求修改文件时，你才能修改系统范围的安全属性文件。然而，在本教程测试中，对于你来说可能不适合修改系统范围的策略文件；我们建议你只是阅读以下内容以了解如何操作，或者安装你自己的私人版本的 JDK 以供教程课程使用。

* * *

安全属性文件位于

+   **Windows**：`*java.home*\lib\security\java.security`

+   **UNIX**：`*java.home*/lib/security/java.security`

*`java.home`*部分表示 JRE 安装的目录。

要修改安全属性文件，请在适合编辑 ASCII 文本文件的编辑器中打开它。然后在以`policy.url.2`开头的行后添加以下行：

+   **Windows**：`**policy.url.3=file:/C:/Test/exampleraypolicy**`

+   **UNIX**：`**policy.url.3=file:${user.home}/test/exampleraypolicy**`

在 UNIX 系统上，您还可以显式指定您的主目录，如

```java
policy.url.3=file:/home/susanj/test/exampleraypolicy

```

接下来，在您的命令窗口中，转到包含`sCount.jar`文件的目录，即`C:\Test`或`~/test`目录。在一行上键入以下命令：

```java
java -Djava.security.manager
        -cp sCount.jar Count C:\TestData\data

```

与第一种方法一样，如果程序仍然报告错误，则可能是策略文件出现问题。使用策略工具检查您在上一步中创建的权限，并更正任何拼写错误或其他错误。

* * *

**重要提示：**在继续之前，您可能希望删除您刚刚在安全属性文件中添加的行（或将其注释掉），因为您可能不希望在不运行教程课程时包含`exampleraypolicy`文件。

* * *
