# 操作小程序网页的 DOM

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/manipulatingDOMFromApplet.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/manipulatingDOMFromApplet.html)

每个网页由一系列嵌套对象组成。这些对象构成了文档对象模型（DOM）。Java 小程序可以使用[`Common DOM API`](https://docs.oracle.com/javase/8/docs/jre/api/plugin/dom/index.html)遍历和修改其父网页的对象。

考虑一个 Java 小程序的示例，它会转储其父网页的内容。

为了遍历和操作 DOM 树，您必须首先获取网页的`Document`对象的引用。您可以使用`com.sun.java.browser.plugin2.DOM`类中的`getDocument`方法来实现。以下是在``DOMDump``小程序的`start`方法中检索`Document`对象引用的代码片段。请查看代码中的内联注释。

```java
public void start() {
    try {
        // use reflection to get document
        Class c =
          Class.forName("com.sun.java.browser.plugin2.DOM");
        Method m = c.getMethod("getDocument",
          new Class[] { java.applet.Applet.class });

        // cast object returned as HTMLDocument;
        // then traverse or modify DOM
        HTMLDocument doc = (HTMLDocument) m.invoke(null,
            new Object[] { this });
        HTMLBodyElement body =
            (HTMLBodyElement) doc.getBody();
        dump(body, INDENT);
    } catch (Exception e) {
        System.out.println("New Java Plug-In not available");
        // In this case, you could fallback to the old
        // bootstrapping mechanism available in the
        // com.sun.java.browser.plugin.dom package
    }
}

```

现在您已经获得了`Document`对象的引用，您可以使用 Common DOM API 遍历和修改 DOM 树。`DOMDump`小程序遍历 DOM 树并将其内容写入 Java 控制台日志。

```java
private void dump(Node root, String prefix) {
    if (root instanceof Element) {
        System.out.println(prefix +
            ((Element) root).getTagName() + 
            " / " + root.getClass().getName());
    } else if (root instanceof CharacterData) {
        String data =
            ((CharacterData) root).getData().trim();
        if (!data.equals("")) {
            System.out.println(prefix +
                "CharacterData: " + data);
        }
    } else {
        System.out.println(prefix +
            root.getClass().getName());
    }
    NamedNodeMap attrs = root.getAttributes();
    if (attrs != null) {
        int len = attrs.getLength();
        for (int i = 0; i < len; i++) {
            Node attr = attrs.item(i);
            System.out.print(prefix + HALF_INDENT +
                "attribute " + i + ": " +
                attr.getNodeName());
            if (attr instanceof Attr) {
                System.out.print(" = " +
                    ((Attr) attr).getValue());
            }
            System.out.println();
        }
    }

    if (root.hasChildNodes()) {
        NodeList children = root.getChildNodes();
        if (children != null) {
            int len = children.getLength();
            for (int i = 0; i < len; i++) {
                dump(children.item(i), prefix +
                    INDENT);
            }
        }
    }
}

```

在浏览器中打开``AppletPage.html``以查看`DOMDump`小程序的运行情况。检查 Java 控制台日志以获取网页 DOM 树的转储。

* * *

**注意：** 如果您看不到小程序运行，请至少安装[Java SE Development Kit (JDK) 6 update 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

* * *

**注意：** 如果您看不到示例运行，您可能需要在浏览器中启用 JavaScript 解释器，以便 Deployment Toolkit 脚本能够正常运行。

* * *

下载源代码以进一步进行实验的*DOM Dump*示例。
