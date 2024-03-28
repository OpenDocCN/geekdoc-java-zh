# 添加剪切、复制和粘贴（CCP）

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/cutpaste.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/cutpaste.html)

到目前为止，我们的讨论主要集中在拖放支持上。然而，将剪切、复制或粘贴（ccp）连接到传输处理程序是一件容易的事情。这需要以下步骤：

+   确保组件上安装了传输处理程序。

+   创建一种方式，通过该方式可以调用`TransferHandler`的 ccp 支持。通常，这涉及向输入和动作映射添加绑定，以便在特定按键响应中调用`TransferHandler`的 ccp 操作。

+   创建 ccp 菜单项和/或按钮。（此步骤是可选的，但建议执行。）对于文本组件，这很容易做到，但对于其他组件，需要更多的工作，因为您需要逻辑来确定在哪个组件上触发操作。查看非文本组件中的 CCP 获取更多信息。

+   决定在哪里执行粘贴。也许在当前选择的上方或下方。在`importData`方法中安装逻辑。

接下来，我们将看一个包含文本组件的剪切和粘贴示例。
