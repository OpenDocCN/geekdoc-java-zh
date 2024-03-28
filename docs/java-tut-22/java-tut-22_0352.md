# 关于 JFC 和 Swing

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/start/about.html`](https://docs.oracle.com/javase/tutorial/uiswing/start/about.html)

JFC 是 Java Foundation Classes 的缩写，它包含了一组用于构建图形用户界面（GUI）和为 Java 应用程序添加丰富的图形功能和交互性的特性。它被定义为包含下表所示的特性。

| 功能 | 描述 |
| --- | --- |
| Swing GUI 组件 | 包括从按钮到分割窗格到表格的所有内容。许多组件能够进行排序、打印和拖放等功能，这只是支持的功能之一。 |
| Pluggable Look-and-Feel Support | Swing 应用程序的外观和感觉是可插拔的，允许选择外观和感觉。例如，同一个程序可以使用 Java 或 Windows 外观和感觉。此外，Java 平台支持 GTK+ 外观和感觉，使得数百种现有的外观和感觉可用于 Swing 程序。许多其他外观和感觉包可以从各种来源获取。 |
| Accessibility API | 允许辅助技术，如屏幕阅读器和盲文显示器，从用户界面获取信息。 |
| Java 2D API | 允许开发人员在应用程序和小程序中轻松地整合高质量的 2D 图形、文本和图像。Java 2D 包括广泛的 API，用于生成和发送高质量的输出到打印设备。 |
| 国际化 | 允许开发人员构建可以与全球用户以其自己的语言和文化习俗交互的应用程序。借助输入法框架，开发人员可以构建接受使用数千个不同字符的语言（如日语、中文或韩语）的文本的应用程序。 |

本教程集中讨论 Swing 组件。我们将帮助您选择适合您的 GUI 的组件，告诉您如何使用它们，并为您提供使用它们的有效背景信息。我们还将讨论其他与 Swing 组件相关的功能。

## 我应该使用哪些 Swing 包？

Swing API 强大、灵活 —— 也是庞大的。Swing API 有 18 个公共包：

| `javax.accessibility` | `javax.swing.plaf` | `javax.swing.text` |
| --- | --- | --- |
| `javax.swing` | `javax.swing.plaf.basic` | `javax.swing.text.html` |
| `javax.swing.border` | `javax.swing.plaf.metal` | `javax.swing.text.html.parser` |
| `javax.swing.colorchooser` | `javax.swing.plaf.multi` | `javax.swing.text.rtf` |
| `javax.swing.event` | `javax.swing.plaf.synth` | `javax.swing.tree` |
| `javax.swing.filechooser` | `javax.swing.table` | `javax.swing.undo` |

幸运的是，大多数程序只使用 API 的一个小子集。本教程为您整理了 API，为您提供常见代码示例，并指导您可能需要的方法和类。本教程中的大部分代码仅使用一个或两个 Swing 包：

+   `javax.swing`

+   `javax.swing.event`（不总是必需的）
