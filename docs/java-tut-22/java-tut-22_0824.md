# 设置策略文件以授予所需的权限

> 原文：[`docs.oracle.com/javase/tutorial/security/tour2/step3.html`](https://docs.oracle.com/javase/tutorial/security/tour2/step3.html)

这一步使用策略工具实用程序打开名为**`examplepolicy`**的策略文件，该文件是在创建策略文件课程中创建的。您将添加一个新的策略条目，允许来自存储`GetProps.class`的目录的代码读取`"user.home"`和`"java.home"`属性值，如下图所示。

![示例策略文件授予`WriteFile`和`GetProps`所需的权限](img/9911200b791be06a6bc39dee7ee2c0e1.png)

步骤如下。

1.  打开策略文件

1.  授予所需的权限

1.  保存策略文件

* * *

**UNIX 用户注意：** 本说明演示了为 Windows 系统创建策略文件。如果您在 UNIX 系统上工作，则步骤完全相同，只有以下区别。

+   您从您的主目录中的`test`目录中检索`examplepolicy`文件。

+   对于授予所需权限的步骤中的**CodeBase** URL，您可以将`file:${user.home}/test/`替换为`file:/C:/Test/`。或者，您可以直接指定您的主目录，而不是引用`"user.home"`属性，如`file:/home/jones/test/`。

* * *
