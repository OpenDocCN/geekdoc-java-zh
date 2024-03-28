# 启动 Policy Tool

> 原文：[`docs.oracle.com/javase/tutorial/security/tour1/wstep1.html`](https://docs.oracle.com/javase/tutorial/security/tour1/wstep1.html)

要启动 Policy Tool，只需在命令行中输入以下内容：

```java
policytool

```

这将打开 Policy Tool 窗口。

每次启动 Policy Tool 时，它会尝试从*用户策略文件*中填充此窗口中的策略信息。用户策略文件默认在您的主目录中命名为`.java.policy`。如果 Policy Tool 找不到用户策略文件，它会发出警告并显示一个空白的 Policy Tool 窗口（一个带有标题和按钮但没有数据的窗口），如下图所示。

![一个空白的 Policy Tool 窗口](img/a36d6173c788f172a5f6335aabbb7cd0.png)

然后，您可以选择打开现有的策略文件或创建新的策略文件。

第一次运行 Policy Tool 时，您会看到空白的 Policy Tool 窗口，因为用户策略文件尚不存在。您可以立即继续创建新的策略文件，如下一步所述。
