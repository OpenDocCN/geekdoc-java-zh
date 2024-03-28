# 创建包含合同的 JAR 文件

> 原文：[`docs.oracle.com/javase/tutorial/security/toolfilex/step1.html`](https://docs.oracle.com/javase/tutorial/security/toolfilex/step1.html)

你需要的第一件事是一个合同文件。你可以下载并使用这个非常基本的示例文件，命名为`contract`。或者你也可以使用其他任何你喜欢的文件。只需确保将文件命名为`contract`，这样它就可以与本课程中指定的命令一起使用。

一旦你有了合同文件，将其放入一个 JAR 文件中。在你的命令窗口中输入以下内容：

```java
jar cvf Contract.jar contract

```

这个命令创建一个名为`Contract.jar`的 JAR 文件，并将`contract`文件放入其中。
