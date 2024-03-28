# 在国际化之前

> 原文：[`docs.oracle.com/javase/tutorial/i18n/intro/before.html`](https://docs.oracle.com/javase/tutorial/i18n/intro/before.html)

假设你已经编写了一个显示三条消息的程序，如下所示：

```java
public class NotI18N {

    static public void main(String[] args) {

        System.out.println("Hello.");
        System.out.println("How are you?");
        System.out.println("Goodbye.");
    }
}

```

你已经决定这个程序需要为居住在法国和德国的人显示相同的消息。不幸的是，你的编程人员不懂多种语言，所以你需要帮助将这些消息翻译成法语和德语。由于翻译人员不是程序员，你需要将消息从源代码中移出，放入文本文件供翻译人员编辑。此外，程序必须足够灵活，以便能够显示其他语言的消息，但目前没有人知道那些语言会是什么。

看起来这个程序需要国际化。
