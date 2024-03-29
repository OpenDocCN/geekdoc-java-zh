# 国际化后

> 原文：[`docs.oracle.com/javase/tutorial/i18n/intro/after.html`](https://docs.oracle.com/javase/tutorial/i18n/intro/after.html)

国际化程序的源代码如下。请注意，消息的文本未包含在软件代码中。

```java
import java.util.*;

public class I18NSample {

    static public void main(String[] args) {

        String language;
        String country;

        if (args.length != 2) {
            language = new String("en");
            country = new String("US");
        } else {
            language = new String(args[0]);
            country = new String(args[1]);
        }

        Locale currentLocale;
        ResourceBundle messages;

        currentLocale = new Locale(language, country);

        messages = ResourceBundle.getBundle("MessagesBundle", currentLocale);
        System.out.println(messages.getString("greetings"));
        System.out.println(messages.getString("inquiry"));
        System.out.println(messages.getString("farewell"));
    }
}

```

要编译和运行此程序，您需要这些源文件：

+   ``I18NSample.java``

+   ``MessagesBundle.properties``

+   ``MessagesBundle_de_DE.properties``

+   ``MessagesBundle_en_US.properties``

+   ``MessagesBundle_fr_FR.properties``
