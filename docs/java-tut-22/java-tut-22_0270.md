# 在浏览器中显示文档

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/browser.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/browser.html)

一个 Java 小程序可以使用 [`java.applet.AppletContext`](https://docs.oracle.com/javase/8/docs/api/java/applet/AppletContext.html) 类中的 `showDocument` 方法在浏览器窗口中加载网页。

以下是 `showDocument` 的两种形式：

```java
public void showDocument(java.net.URL *url*)
public void showDocument(java.net.URL *url*, String *targetWindow*)

```

`showDocument` 的单参数形式简单地指示浏览器显示指定 URL 的文档，而不指定显示文档的窗口。

`showDocument` 的双参数形式允许您指定显示文档的窗口或 HTML 框架。第二个参数可以有以下值之一：

+   `"_blank"` – 在一个新的、无名称的窗口中显示文档。

+   `"*windowName*"` – 在名为 *windowName* 的窗口中显示文档。如有必要，将创建此窗口。

+   `"_self"` – 在包含小程序的窗口和框架中显示文档。

+   `"_parent"` – 在小程序框架的父框架中显示文档。如果小程序框架没有父框架，则与 `"_self"` 相同。

+   `"_top"` – 在顶层框架中显示文档。如果小程序框架是顶层框架，则与 `"_self"` 相同。

* * *

**注意：** 在这个讨论中，*frame* 指的不是一个 Swing `JFrame`，而是浏览器窗口内的 HTML 框架。

* * *

以下小程序使您可以尝试 `showDocument` 两种形式的每个参数。该小程序打开一个窗口，让您输入 URL 并选择 `targetWindow` 参数的选项。当您按下 Return 键或点击显示文档按钮时，小程序调用 `showDocument`。

//&lt;![CDATA[ var attributes = { code:'ShowDocument.class', archive:'examples/dist/applet_ShowDocument/applet_ShowDocument.jar', width:200, height:30} ; var parameters = { permissions:'sandbox' } ; deployJava.runApplet(attributes, parameters, '1.4'); //]]&gt;

* * *

**注意：** 如果您看不到示例运行，可能需要在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

以下是调用 `showDocument` 的小程序代码。这是整个程序，`ShowDocument`。

```java
        *...//In an Applet subclass:*
        urlWindow = new URLWindow(getAppletContext());
        . . .

class URLWindow extends Frame {
    ...
    public URLWindow(AppletContext appletContext) {
        ...
        this.appletContext = appletContext;
        ...
    }
    ...
    public boolean action(Event event, Object o) {
        ...
            String urlString =
                */* user-entered string */*;
            URL url = null;
            try {
                url = new URL(urlString);
            } catch (MalformedURLException e) {
                *...//Inform the user and return...*
            }

            if (url != null) {
                if (*/* user doesn't want to specify
                           the window */*) {
                    appletContext.showDocument(url);
                } else {
                    appletContext.showDocument(url,
                        */* user-specified window */*);
                }
            }
        ...

```

下载源代码 以进一步尝试 *Show Document* 示例。
