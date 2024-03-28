# 创建 URL

> 原文：[`docs.oracle.com/javase/tutorial/networking/urls/creatingUrls.html`](https://docs.oracle.com/javase/tutorial/networking/urls/creatingUrls.html)

创建 `URL` 对象的最简单方法是从表示 URL 地址的人类可读形式的 `String` 创建。这通常是另一个人将用于 URL 的形式。在您的 Java 程序中，您可以使用包含此文本的 `String` 来创建 `URL` 对象：

```java
URL myURL = new URL("http://example.com/");

```

上面创建的 `URL` 对象表示一个*绝对 URL*。绝对 URL 包含到达所需资源的所有必要信息。您还可以从*相对 URL*地址创建 `URL` 对象。

## 创建相对于另一个的 URL

相对 URL 只包含足够的信息，以便相对于（或在另一个 URL 的上下文中）到达资源。

相对 URL 规范通常在 HTML 文件中使用。例如，假设您编写了一个名为 `JoesHomePage.html` 的 HTML 文件。在此页面中，有指向其他页面 `PicturesOfMe.html` 和 `MyKids.html` 的链接，这些页面位于与 `JoesHomePage.html` 相同的机器和目录中。从 `JoesHomePage.html` 到 `PicturesOfMe.html` 和 `MyKids.html` 的链接可以像文件名一样指定，如下所示：

```java
<a href="PicturesOfMe.html">Pictures of Me</a>
<a href="MyKids.html">Pictures of My Kids</a>

```

这些 URL 地址是*相对 URL*。也就是说，这些 URL 是相对于包含它们的文件——`JoesHomePage.html`来指定的。

在您的 Java 程序中，您可以从相对 URL 规范创建一个 `URL` 对象。例如，假设您在站点 `example.com` 知道两个 URL：

```java
http://example.com/pages/page1.html
http://example.com/pages/page2.html

```

您可以相对于它们的共同基本 URL `http://example.com/pages/` 创建这些页面的 `URL` 对象，如下所示：

```java
URL myURL = new URL("http://example.com/pages/");
URL page1URL = new URL(myURL, "page1.html");
URL page2URL = new URL(myURL, "page2.html");

```

此代码片段使用了一个 `URL` 构造函数，让您可以从另一个 `URL` 对象（基础）和相对 URL 规范创建一个 `URL` 对象。此构造函数的一般形式是：

```java
URL(URL *baseURL*, String *relativeURL*)

```

第一个参数是指定新 `URL` 的基础的 `URL` 对象。第二个参数是指定相对于基础的资源名称的其余部分的 `String`。如果 `baseURL` 为 null，则此构造函数将 `relativeURL` 视为绝对 URL 规范。相反，如果 `relativeURL` 是绝对 URL 规范，则构造函数将忽略 `baseURL`。

这个构造函数也适用于为文件中的命名锚点（也称为引用）创建 `URL` 对象。例如，假设 `page1.html` 文件在文件底部有一个名为 `BOTTOM` 的命名锚点。您可以使用相对 URL 构造函数为其创建一个 `URL` 对象，如下所示：

```java
URL page1BottomURL = new URL(page1URL,"#BOTTOM");

```

## 其他 URL 构造函数

`URL` 类提供了另外两个用于创建 `URL` 对象的构造函数。当您使用具有主机名、文件名、端口号和 URL 资源名称部分中的引用组件的 URL（如 HTTP URL）时，这些构造函数很有用。当您没有包含完整 URL 规范的 String，但您知道 URL 的各个组件时，这两个构造函数很有用。

例如，假设您设计一个类似文件浏览面板的网络浏览面板，允许用户选择协议、主机名、端口号和文件名。您可以从面板的组件构造一个`URL`。第一个构造函数从协议、主机名和文件名创建一个`URL`对象。以下代码片段创建一个`URL`到`example.com`站点的`page1.html`文件：

```java
new URL("http", "example.com", "/pages/page1.html");

```

这等同于

```java
new URL("http://example.com/pages/page1.html");

```

第一个参数是协议，第二个是主机名，最后一个是文件的路径名。请注意，文件名包含一个斜杠在开头。这表示文件名是从主机的根目录指定的。

最终的`URL`构造函数将端口号添加到前一个构造函数中使用的参数列表中：

```java
URL gamelan = new URL("http", "example.com", 80, "pages/page1.html"); 

```

这将为以下 URL 创建一个`URL`对象：

```java
http://example.com:80/pages/page1.html

```

如果使用这些构造函数之一构造`URL`对象，您可以通过使用`URL`对象的`toString`方法或等效的`toExternalForm`方法获得包含完整 URL 地址的`String`。

## 具有特殊字符的 URL 地址

一些 URL 地址包含特殊字符，例如空格字符。就像这样：

```java
http://example.com/hello world/

```

在将这些字符传递给 URL 构造函数之前，它们需要进行编码以使其合法。

```java
URL url = new URL("http://example.com/hello%20world");

```

在这个例子中编码特殊字符很容易，因为只有一个字符需要编码，但是对于具有多个这些字符的 URL 地址，或者在编写代码时不确定需要访问哪些 URL 地址时，您可以使用[`java.net.URI`](https://docs.oracle.com/javase/8/docs/api/java/net/URI.html)类的多参数构造函数来自动处理编码。

```java
URI uri = new URI("http", "example.com", "/hello world/", "");

```

然后将 URI 转换为 URL。

```java
URL url = uri.toURL();

```

## MalformedURLException

四个`URL`构造函数中的每一个如果构造函数的参数引用`null`或未知协议，则会抛出`MalformedURLException`。通常，您希望通过将 URL 构造函数语句嵌入到`try`/`catch`对中来捕获和处理此异常，就像这样：

```java
try {
    URL myURL = new URL(...);
} 
catch (MalformedURLException e) {
    // *exception handler code here*
    // ...
}

```

有关处理异常的信息，请参阅异常。

* * *

**注意：**

`URL`是“一次写入”对象。一旦创建了`URL`对象，就无法更改其任何属性（协议、主机名、文件名或端口号）。

* * *
