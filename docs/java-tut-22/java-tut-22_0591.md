# 准备使用 ResourceBundle

> 原文：[`docs.oracle.com/javase/tutorial/i18n/resbundle/prepare.html`](https://docs.oracle.com/javase/tutorial/i18n/resbundle/prepare.html)

## 识别与地区相关的对象

如果你的应用程序有用户界面，它包含许多与地区相关的对象。要开始，你应该查看源代码，寻找随`Locale`变化的对象。你的列表可能包括从以下类实例化的对象：

+   `字符串`

+   `图像`

+   `颜色`

+   `AudioClip`

你会注意到这个列表不包含代表数字、日期、时间或货币的对象。这些对象的显示格式随`Locale`变化，但对象本身不会变化。例如，你根据`Locale`格式化一个`Date`，但无论`Locale`如何，你都使用相同的`Date`对象。你不需要将这些对象隔离在`ResourceBundle`中，而是使用特殊的区域敏感格式化类对它们进行格式化。你将在日期和时间部分的格式化课程中学习如何做到这一点。

通常情况下，存储在`ResourceBundle`中的对象是预定义的，并随产品一起提供。这些对象在程序运行时不会被修改。例如，你应该将`Menu`标签存储在`ResourceBundle`中，因为它是与地区相关的，在程序会话期间不会更改。然而，你不应该将用户在`TextField`中输入的`String`对象隔离在`ResourceBundle`中。这样的`String`数据可能每天都会有所变化。它是特定于程序会话的，而不是程序运行的`Locale`。

通常，你需要在`ResourceBundle`中隔离的大多数对象都是`String`对象。然而，并非所有的`String`对象都是与地区相关的。例如，如果一个`String`是进程间通信使用的协议元素，它就不需要本地化，因为最终用户永远不会看到它。

是否本地化某些`String`对象的决定并不总是明确的。日志文件是一个很好的例子。如果一个日志文件由一个程序编写并由另一个程序读取，那么两个程序都将使用日志文件作为通信的缓冲区。假设最终用户偶尔检查此日志文件的内容。那么日志文件应该本地化吗？另一方面，如果最终用户很少检查日志文件，则翻译的成本可能不值得。你是否本地化此日志文件的决定取决于许多因素：程序设计、易用性、翻译成本和可支持性。

## 组织 ResourceBundle 对象

你可以根据包含的对象的类别组织你的`ResourceBundle`对象。例如，你可能希望将订单输入窗口的所有 GUI 标签加载到一个名为`OrderLabelsBundle`的`ResourceBundle`中。使用多个`ResourceBundle`对象有几个优点：

+   你的代码更易于阅读和维护。

+   你将避免巨大的`ResourceBundle`对象，这可能需要太长时间加载到内存中。

+   当需要时，您可以通过仅加载每个`ResourceBundle`来减少内存使用量。
