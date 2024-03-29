# 课程：执行自定义绘制

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/painting/index.html`](https://docs.oracle.com/javase/tutorial/uiswing/painting/index.html)

这节课描述了 Swing 中的自定义绘制。许多程序可以很好地运行而无需编写自己的绘制代码；它们将简单地使用 Swing API 中已经可用的标准 GUI 组件。但是，如果您需要对图形的绘制方式进行特定控制，那么这节课就是为您准备的。我们将通过创建一个简单的 GUI 应用程序来探讨自定义绘制，该应用程序根据用户的鼠标活动绘制形状。通过故意保持其设计简单，我们可以专注于底层绘制概念，这将与您将来开发的其他 GUI 应用程序相关联。

这节课在构建演示应用程序时逐步解释每个概念。它尽可能快地呈现代码，最小化背景阅读量。在 Swing 中进行自定义绘制类似于在 AWT 中进行自定义绘制，但由于我们不建议完全使用 AWT 编写应用程序，因此这里不特别讨论其绘制机制。您可能会发现在阅读本课程后阅读文章《[AWT 和 Swing 中的绘制](http://www.oracle.com/technetwork/java/painting-140037.html)》中的深入讨论很有用。
