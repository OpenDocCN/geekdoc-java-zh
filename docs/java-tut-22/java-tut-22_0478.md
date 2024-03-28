# 解决常见数据传输问题

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/problems.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/problems.html)

当使用数据传输时，可能会遇到一些问题。

**问题：** 我的拖动手势识别器在与表格/列表/树/文本一起使用时无法正常工作。

不要在这些组件上使用自己的拖动手势识别器。使用`setDragEnabled(true)`和`TransferHandler`。

**问题：** 我无法将数据放置到我的空`JTable`中。

你需要在表格上调用`setFillsViewportHeight(true)`。查看空表格拖放获取更多信息。
