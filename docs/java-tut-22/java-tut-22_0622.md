# 单词边界

> 原文：[`docs.oracle.com/javase/tutorial/i18n/text/word.html`](https://docs.oracle.com/javase/tutorial/i18n/text/word.html)

您调用`getWordIterator`方法来实例化一个检测单词边界的`BreakIterator`：

```java
BreakIterator wordIterator =
    BreakIterator.getWordInstance(currentLocale);

```

当您的应用程序需要对单词执行操作时，您会想要创建这样一个`BreakIterator`。这些操作可能是常见的单词处理功能，如选择、剪切、粘贴和复制。或者，您的应用程序可能会搜索单词，并且必须能够区分整个单词和简单字符串。

当`BreakIterator`分析单词边界时，它区分单词和不属于单词的字符。这些字符包括空格、制表符、标点符号和大多数符号，在两侧都有单词边界。

接下来的示例来自程序`BreakIteratorDemo`，标记了一些文本中的单词边界。该程序创建了`BreakIterator`，然后调用`markBoundaries`方法：

```java
Locale currentLocale = new Locale ("en","US");

BreakIterator wordIterator =
    BreakIterator.getWordInstance(currentLocale);

String someText = "She stopped. " +
    "She said, \"Hello there,\" and then went " +
    "on.";

markBoundaries(someText, wordIterator);

```

`markBoundaries`方法在`BreakIteratorDemo.java`中定义。该方法通过在目标字符串下方打印插入符号(^)来标记边界。在接下来的代码中，请注意`while`循环，其中`markBoundaries`通过调用`next`方法扫描字符串：

```java
static void markBoundaries(String target, BreakIterator iterator) {

    StringBuffer markers = new StringBuffer();
    markers.setLength(target.length() + 1);
    for (int k = 0; k < markers.length(); k++) {
        markers.setCharAt(k,' ');
    }

    iterator.setText(target);
    int boundary = iterator.first();

    while (boundary != BreakIterator.DONE) {
        markers.setCharAt(boundary,'^');
        boundary = iterator.next();
    }

    System.out.println(target);
    System.out.println(markers);
}

```

`markBoundaries`方法的输出如下。请注意插入符号(^)相对于标点符号和空格的位置：

```java
She stopped.  She said, "Hello there," and then
^  ^^      ^^ ^  ^^   ^^^^    ^^    ^^^^  ^^   ^

went on.
^   ^^ ^^

```

`BreakIterator`类使得从文本中选择单词变得容易。您不必编写自己的处理各种语言标点规则的例程；`BreakIterator`类会为您处理这些。

以下示例中的`extractWords`方法提取并打印给定字符串的单词。请注意，该方法使用`Character.isLetterOrDigit`来避免打印包含空格字符的“单词”。

```java
static void extractWords(String target, BreakIterator wordIterator) {

    wordIterator.setText(target);
    int start = wordIterator.first();
    int end = wordIterator.next();

    while (end != BreakIterator.DONE) {
        String word = target.substring(start,end);
        if (Character.isLetterOrDigit(word.charAt(0))) {
            System.out.println(word);
        }
        start = end;
        end = wordIterator.next();
    }
}

```

`BreakIteratorDemo`程序调用`extractWords`，将其传递给前面示例中使用的相同目标字符串。`extractWords`方法打印出以下单词列表：

```java
She
stopped
She
said
Hello
there
and
then
went
on

```
