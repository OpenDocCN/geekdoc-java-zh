# 可用区域设置的识别

> 原文：[`docs.oracle.com/javase/tutorial/i18n/locale/identify.html`](https://docs.oracle.com/javase/tutorial/i18n/locale/identify.html)

您可以使用任何有效的语言和国家代码组合创建`Locale`，但这并不意味着您可以使用它。请记住，`Locale`对象只是一个标识符。您将`Locale`对象传递给其他对象，然后这些对象才会执行实际工作。这些其他对象，我们称之为区域敏感对象，不知道如何处理所有可能的`Locale`定义。

要找出区域敏感类识别的哪些类型的`Locale`定义，您可以调用`getAvailableLocales`方法。例如，要找出`DateFormat`类支持哪些`Locale`定义，您可以编写以下类似的例程：

```java
import java.util.*;
import java.text.*;

public class Available {
    static public void main(String[] args) {
        Locale list[] = DateFormat.getAvailableLocales();
        for (Locale aLocale : list) {
            System.out.println(aLocale.toString());
        }
    }
}

```

请注意，`toString`返回的`String`包含由下划线分隔的语言和国家代码：

```java
ar_EG
be_BY
bg_BG
ca_ES
cs_CZ
da_DK
de_DE
...

```

如果您想向最终用户显示`Locale`名称列表，您应该向他们显示比`toString`返回的语言和国家代码更容易理解的内容。相反，您可以调用`Locale.getDisplayName`方法，该方法检索`Locale`对象的本地化`String`。例如，当在前面的代码中用`getDisplayName`替换`toString`时，程序会打印以下行：

```java
Arabic (Egypt)
Belarussian (Belarus)
Bulgarian (Bulgaria)
Catalan (Spain)
Czech (Czech Republic)
Danish (Denmark)
German (Germany)
...

```

根据 Java 平台的不同实现，您可能会看到不同的区域设置列表。
