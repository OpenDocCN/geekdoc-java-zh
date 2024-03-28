# 句子边界

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/sentence.html`](https://docs.oracle.com/javase/tutorial/i18n/text/sentence.html)

你可以使用`BreakIterator`来确定句子边界。首先通过`getSentenceInstance`方法创建一个`BreakIterator`：

```java
BreakIterator sentenceIterator =
    BreakIterator.getSentenceInstance(currentLocale);

```

为了显示句子边界，程序使用了`markBoundaries`方法，该方法在单词边界一节中有讨论。`markBoundaries`方法在字符串下方打印插入符号(^)来指示边界位置。以下是一些示例：

```java
She stopped.  She said, "Hello there," and then went on.
^             ^                                         ^

He's vanished!  What will we do?  It's up to us.
^               ^                 ^             ^

Please add 1.5 liters to the tank.
^                                 ^

```
