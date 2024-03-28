# 教程：全屏独占模式 API

> 原文：[`docs.oracle.com/javase/tutorial/extra/fullscreen/index.html`](https://docs.oracle.com/javase/tutorial/extra/fullscreen/index.html)

## *作者：迈克尔·马塔克*

您想在 Java 开发环境中使用高性能图形吗？您一直想编写游戏，但是您的图像移动得不够快吗？您的幻灯片程序因为无法控制用户的显示分辨率而无法正常工作吗？如果您一直在问这些问题，那么在 1.4 版本中引入的全屏独占模式 API 可能是您正在寻找的东西。

全屏独占模式

全屏独占模式是一个强大的新功能，它使您能够暂停窗口系统，以便直接在屏幕上进行绘制。

显示模式

本节描述了如何选择和设置显示模式。还讨论了为什么首先要设置显示模式。

被动渲染 vs. 主动渲染

本节讨论了被动渲染和主动渲染的优点。例如，在主事件循环中使用`paint`方法绘制是被动的，而在自己的线程中渲染是主动的。还列出了主动渲染的技巧。

双缓冲和页面翻转

本节解释了双缓冲并介绍了页面翻转，这是全屏独占模式中可用的双缓冲技术。

BufferStrategy 和 BufferCapabilities

本节介绍了`java.awt.image.BufferStrategy`类，该类允许您在不知道使用的缓冲区数量或显示它们的技术的情况下绘制到表面和组件。本节还回顾了`java.awt.BufferCapabilities`类，该类可以帮助您确定图形设备的功能。

示例

本页面列出了几个全屏独占模式示例。
