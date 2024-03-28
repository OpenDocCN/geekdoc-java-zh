# 课程：并发编程

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/index.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)

计算机用户认为他们的系统可以同时执行多项任务是理所当然的。他们认为他们可以在一个文字处理器中继续工作，同时其他应用程序可以下载文件，管理打印队列和流式传输音频。甚至单个应用程序通常也被期望同时执行多项任务。例如，流式传输音频应用程序必须同时从网络上读取数字音频，解压缩它，管理播放和更新显示。即使文字处理器也应该始终准备好响应键盘和鼠标事件，无论它是在重新格式化文本还是更新显示。能够执行这些操作的软件被称为*并发*软件。

Java 平台从头开始就设计用于支持并发编程，在 Java 编程语言和 Java 类库中具有基本的并发支持。自 5.0 版本以来，Java 平台还包括高级并发 API。本课程介绍了平台的基本并发支持，并总结了`java.util.concurrent`包中的一些高级 API。
