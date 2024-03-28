# 显示放置位置

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/dnd/showdroploc.html`](https://docs.oracle.com/javase/tutorial/uiswing/dnd/showdroploc.html)

通常在拖放操作期间，组件在可以接受数据时会给出视觉反馈。它可能会突出显示放置位置，或者显示插入位置的插入符或水平线。当组件的`TransferHandler`的`canImport`方法返回 true 时，Swing 会渲染放置位置。

要在程序中控制这一点，您可以使用[`setShowDropLocation`](https://docs.oracle.com/javase/8/docs/api/javax/swing/TransferHandler.TransferSupport.html#setShowDropLocation-boolean-)方法。调用此方法并传入`true`会导致始终显示放置位置的视觉反馈，即使放置操作不被接受。调用此方法并传入`false`会阻止任何视觉反馈，即使放置操作将被接受。您始终要从`canImport`中调用此方法。

演示 - LocationSensitiveDemo 页面包括一个下拉框，让您可以选择始终显示放置位置、从不显示放置位置或默认行为。但首先我们将讨论位置敏感的放置。
