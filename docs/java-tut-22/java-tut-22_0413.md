# 问题和练习：Swing 中的并发

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/QandE/questions-concurrency.html`](https://docs.oracle.com/javase/tutorial/uiswing/QandE/questions-concurrency.html)

## 问题

1.  对于以下每个任务，请指定应在哪个线程中执行以及原因。

    +   初始化 GUI。

    +   加载一个大文件。

    +   调用[`javax.swing.JComponent.setFont`](https://docs.oracle.com/javase/8/docs/api/javax/swing/JComponent.html#setFont-java.awt.Font-)来更改组件的字体。

    +   调用[`javax.swing.text.JTextComponent.setText`](https://docs.oracle.com/javase/8/docs/api/javax/swing/text/JTextComponent.html#setText-java.lang.String-)来更改组件的文本。

1.  一组线程不用于前面问题中提到的任何任务。命名此线程并解释为什么其应用如此有限。

1.  `SwingWorker`有两个类型参数。解释这些类型参数如何使用，以及为什么通常不重要。

## 练习

1.  修改``Flipper``示例，使其在“抛硬币”之间暂停 5 秒。如果用户点击“取消”，则立即终止抛硬币循环。

检查你的答案。
