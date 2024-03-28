# 国际化示例程序

> 原文：[`docs.oracle.com/javase/tutorial/i18n/intro/steps.html`](https://docs.oracle.com/javase/tutorial/i18n/intro/steps.html)

如果你查看国际化的源代码，你会注意到硬编码的英文消息已经被移除。因为消息不再是硬编码的，而且语言代码在运行时指定，同一个可执行文件可以在全球范围内分发。本地化不需要重新编译。该程序已经国际化。

你可能想知道消息的文本发生了什么变化，或者语言和国家代码的含义是什么。别担心。当你逐步学习国际化示例程序的过程中，你会了解这些概念。

## 1\. 创建属性文件

一个属性文件存储了关于程序或环境特性的信息。属性文件是纯文本格式。你可以用几乎任何文本编辑器创建这个文件。

在示例中，属性文件存储了要显示的可翻译消息的文本。在程序国际化之前，这些文本的英文版本是硬编码在`System.out.println`语句中的。默认的属性文件名为`MessagesBundle.properties`，包含以下内容：

```java
greetings = Hello
farewell = Goodbye
inquiry = How are you?

```

现在消息已经在一个属性文件中，它们可以被翻译成各种语言。不需要对源代码进行任何更改。法语翻译者创建了一个名为`MessagesBundle_fr_FR.properties`的属性文件，其中包含以下内容：

```java
greetings = Bonjour.
farewell = Au revoir.
inquiry = Comment allez-vous?

```

注意等号右侧的值已经被翻译，但左侧的键没有改变。这些键不能改变，因为当你的程序获取翻译后的文本时会引用这些键。

属性文件的名称很重要。例如，`MessagesBundle_fr_FR.properties`文件的名称包含了`fr`语言代码和`FR`国家代码。在创建`Locale`对象时也会使用这些代码。

## 2\. 定义 Locale

`Locale`对象标识特定的语言和国家。以下语句定义了一个`Locale`，其中语言为英语，国家为美国：

```java
aLocale = new Locale("en","US");

```

下一个示例创建了法语语言在加拿大和法国的`Locale`对象：

```java
caLocale = new Locale("fr","CA");
frLocale = new Locale("fr","FR");

```

该程序是灵活的。程序不再使用硬编码的语言和国家代码，而是在运行时从命令行获取它们：

```java
String language = new String(args[0]);
String country = new String(args[1]);
currentLocale = new Locale(language, country);

```

`Locale`对象只是标识符。在定义了一个`Locale`之后，你可以将它传递给执行有用任务的其他对象，比如格式化日期和数字。这些对象是*区域敏感*的，因为它们的行为根据`Locale`的不同而变化。`ResourceBundle`就是一个区域敏感的对象的例子。

## 3\. 创建 ResourceBundle

`ResourceBundle`对象包含特定于区域设置的对象。你可以使用`ResourceBundle`对象来隔离与区域设置相关的数据，比如可翻译的文本。在示例程序中，`ResourceBundle`由包含我们想要显示的消息文本的属性文件支持。

`ResourceBundle`的创建方式如下：

```java
messages = ResourceBundle.getBundle("MessagesBundle", currentLocale);

```

传递给`getBundle`方法的参数标识将要访问的属性文件。第一个参数`MessagesBundle`指的是这一系列属性文件：

```java
MessagesBundle_en_US.properties
MessagesBundle_fr_FR.properties
MessagesBundle_de_DE.properties

```

`Locale`是`getBundle`的第二个参数，指定了选择哪个`MessagesBundle`文件。创建`Locale`时，语言代码和国家代码被传递给其构造函数。请注意，语言和国家代码在属性文件的名称中跟随`MessagesBundle`。

现在你只需要从`ResourceBundle`中获取翻译后的消息即可。

## 4\. 从 ResourceBundle 中获取文本

属性文件包含键值对。值包含了程序将显示的翻译文本。在从`ResourceBundle`中获取翻译后的消息时，你需要使用`getString`方法指定键。例如，要检索由`greetings`键标识的消息，你可以这样调用`getString`：

```java
String msg1 = messages.getString("greetings");

```

示例程序使用了键`greetings`，因为它反映了消息的内容，但也可以使用另一个`String`，比如`s1`或`msg1`。只需记住，键是硬编码在程序中的，必须存在于属性文件中。如果你的翻译人员意外修改了属性文件中的键，`getString`将无法找到消息。

## 结论

这就是全部。正如你所看到的，国际化一个程序并不太困难。它需要一些规划和一点额外的编码，但好处是巨大的。为了让你了解国际化过程的概况，本课程中的示例程序被故意保持简单。在接下来的课程中，你将了解 Java 编程语言更高级的国际化特性。
