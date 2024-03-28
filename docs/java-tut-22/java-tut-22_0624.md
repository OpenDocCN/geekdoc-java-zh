# 行边界

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/line.html`](https://docs.oracle.com/javase/tutorial/i18n/text/line.html)

应用程序格式化文本或执行换行操作必须找到潜在的换行位置。您可以使用使用`getLineInstance`方法创建的`BreakIterator`来找到这些换行位置或边界：

```java
BreakIterator lineIterator =
    BreakIterator.getLineInstance(currentLocale);

```

这个`BreakIterator`确定字符串中文本可以断开以继续到下一行的位置。`BreakIterator`检测到的位置是潜在的换行位置。在屏幕上显示的实际换行可能不同。

接下来的两个示例使用`BreakIteratorDemo.java`的`markBoundaries`方法来显示`BreakIterator`检测到的行边界。`markBoundaries`方法通过在目标字符串下方打印插入符号(^)来指示行边界。

根据`BreakIterator`，在一系列空格字符（空格、制表符、换行符）的终止后发生行边界。在下面的示例中，请注意您可以在检测到的任何边界处断开行：

```java
She stopped.  She said, "Hello there," and then went on.
^   ^         ^   ^     ^      ^     ^ ^   ^    ^    ^  ^

```

潜在的换行位置也会在连字符后立即发生：

```java
There are twenty-four hours in a day.
^     ^   ^      ^    ^     ^  ^ ^   ^

```

下一个示例将长文本字符串分成固定长度的行，使用名为`formatLines`的方法。该方法使用`BreakIterator`来定位潜在的换行位置。`formatLines`方法简短、简单，并且由于使用了`BreakIterator`，与语言环境无关。以下是源代码：

```java
static void formatLines(
    String target, int maxLength,
    Locale currentLocale) {

    BreakIterator boundary = BreakIterator.
        getLineInstance(currentLocale);
    boundary.setText(target);
    int start = boundary.first();
    int end = boundary.next();
    int lineLength = 0;

    while (end != BreakIterator.DONE) {
        String word = target.substring(start,end);
        lineLength = lineLength + word.length();
        if (lineLength >= maxLength) {
            System.out.println();
            lineLength = word.length();
        }
        System.out.print(word);
        start = end;
        end = boundary.next();
    }
}

```

`BreakIteratorDemo`程序调用`formatLines`方法如下：

```java
String moreText =
    "She said, \"Hello there,\" and then " +
    "went on down the street. When she stopped " +
    "to look at the fur coats in a shop + "
    "window, her dog growled. \"Sorry Jake,\" " +
    "she said. \"I didn't know you would take " +
    "it personally.\"";

formatLines(moreText, 30, currentLocale);

```

调用`formatLines`的输出为：

```java
She said, "Hello there," and
then went on down the
street. When she stopped to
look at the fur coats in a
shop window, her dog
growled. "Sorry Jake," she
said. "I didn't know you
would take it personally."

```
