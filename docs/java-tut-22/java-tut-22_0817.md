# 设置策略文件以授予所需权限

> 原文：[`docs.oracle.com/javase/tutorial/security/tour1/step2.html`](https://docs.oracle.com/javase/tutorial/security/tour1/step2.html)

策略文件是一个 ASCII 文本文件，可以通过文本编辑器或本节中演示的图形化策略工具来编写。策略工具可以节省您的输入时间，消除您需要了解策略文件所需语法的需求，从而减少错误。

本课程使用策略工具创建名为 **`examplepolicy`** 的策略文件，在其中您将添加一个 *策略条目*，授予来自 **`examples`** 目录的代码写入权限。

按照以下步骤创建和修改您的新策略文件：

1.  启动策略工具

1.  授予所需权限

1.  保存策略文件

* * *

**UNIX 用户注意事项：** 这些步骤说明了如何为 Windows 系统创建策略文件。如果您在 UNIX 系统上工作，步骤完全相同。当文本中说要将策略文件存储在 `C:\Test` 目录中时，您可以将其存储在其他目录中。本课程中的示例假定您将其存储在 `~/test` 目录中。

* * *
