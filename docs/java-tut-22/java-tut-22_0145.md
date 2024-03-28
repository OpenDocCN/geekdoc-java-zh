# 问题和练习：创建和使用包

> 原文：[`docs.oracle.com/javase/tutorial/java/package/QandE/packages-questions.html`](https://docs.oracle.com/javase/tutorial/java/package/QandE/packages-questions.html)

## 问题

假设你已经编写了一些类。突然间，你决定将它们分成三个包，如下表所列。此外，假设这些类当前位于默认包中（没有`package`语句）。

目标包

| 包名称 | 类名称 |
| --- | --- |
| `mygame.server` | `Server` |
| `mygame.shared` | `Utilities` |
| `mygame.client` | `Client` |

1.  你需要在每个源文件中添加哪行代码才能将每个类放在正确的包中？

1.  为了遵循目录结构，你需要在开发目录中创建一些子目录，并将源文件放在正确的子目录中。你需要创建哪些子目录？每个源文件应放在哪个子目录中？

1.  你认为你需要对源文件进行其他任何更改才能使它们正确编译吗？如果需要，是什么？

## 练习

下载列在这里的源文件。

+   `Client`

+   `Server`

+   `Utilities`

1.  使用刚刚下载的源文件实现你在问题 1 到 3 中提出的更改。

1.  编译修改后的源文件。（*提示：如果你是从命令行调用编译器（而不是使用构建工具），请从包含你刚刚创建的`mygame`目录的目录中调用编译器。*）

检查你的答案。
