# 消息

> 原文：[`docs.oracle.com/javase/tutorial/i18n/format/messageintro.html`](https://docs.oracle.com/javase/tutorial/i18n/format/messageintro.html)

我们都喜欢使用让我们了解正在发生什么的程序。通常，让我们保持了解的程序通过显示状态和错误消息来实现。当然，这些消息需要被翻译，以便全球的最终用户能够理解。隔离特定区域数据部分讨论了可翻译的文本消息。通常，在将消息`String`移入`ResourceBundle`后，你就完成了。然而，如果在消息中嵌入了变量数据，你需要采取一些额外的步骤来准备翻译。

*复合消息*包含变量数据。在下面的复合消息列表中，变量数据被划线标出：

```java
The disk named MyDisk contains 300 files.
The current balance of account #34-09-222 is $2,745.72.
405,390 people have visited your website since January 1, 2009.
Delete all files older than 120 days.

```

你可能会尝试通过连接短语和变量来构建前述列表中的最后一条消息，如下所示：

```java
double numDays;
ResourceBundle msgBundle;
// ...
String message = msgBundle.getString(
                     "deleteolder" +
                     numDays.toString() +
                     msgBundle.getString("days"));

```

这种方法在英语中效果很好，但对于动词出现在句子末尾的语言来说，这种方法不适用。因为这条消息的词序是硬编码的，你的本地化人员将无法为所有语言创建语法正确的翻译。

如果需要使用复合消息，如何使你的程序可本地化？你可以通过使用`MessageFormat`类来实现，这是本节的主题。

* * *

**注意：**

复合消息很难翻译，因为消息文本是分散的。如果使用复合消息，本地化将需要更长的时间和更多的成本。因此，只有在必要时才应使用复合消息。

* * *

## 处理复合消息

复合消息可能包含多种类型的变量：日期、时间、字符串、数字、货币和百分比。为了以与区域无关的方式格式化复合消息，你需要构建一个模式，然后应用到`MessageFormat`对象上。

## 处理复数形式

如果可能同时存在复数和单数形式的词汇，消息中的词汇通常会有所变化。通过`ChoiceFormat`类，你可以将数字映射到一个词或短语，从而构建语法正确的消息。
