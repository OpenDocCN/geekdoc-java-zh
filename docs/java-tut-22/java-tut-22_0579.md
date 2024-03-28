# 运行示例程序

> 原文：[`docs.oracle.com/javase/tutorial/i18n/intro/run.html`](https://docs.oracle.com/javase/tutorial/i18n/intro/run.html)

国际化程序是灵活的；它允许最终用户在命令行上指定语言和国家。在下面的示例中，语言代码是`fr`（法语），国家代码是`FR`（法国），因此程序会以法语显示消息：

```java
% java I18NSample fr FR
Bonjour.
Comment allez-vous?
Au revoir.

```

在下一个示例中，语言代码是`en`（英语），国家代码是`US`（美国），因此程序会以英语显示消息：

```java
% java I18NSample en US
Hello.
How are you?
Goodbye.

```
