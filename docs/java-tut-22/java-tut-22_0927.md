# 解析 URL

> 原文：[`docs.oracle.com/javase/tutorial/networking/urls/urlInfo.html`](https://docs.oracle.com/javase/tutorial/networking/urls/urlInfo.html)

`URL`类提供了几种方法，让您可以查询`URL`对象。您可以使用这些访问器方法从 URL 中获取协议、权限、主机名、端口号、路径、查询、文件名和引用：

**`getProtocol`**

返回 URL 的协议标识符组件。

**`getAuthority`**

返回 URL 的权限组件。

**`getHost`**

返回 URL 的主机名组件。

**`getPort`**

返回 URL 的端口号组件。`getPort`方法返回一个整数，即端口号。如果端口未设置，`getPort`返回-1。

**`getPath`**

返回此 URL 的路径组件。

**`getQuery`**

返回此 URL 的查询组件。

**`getFile`**

返回 URL 的文件名组件。`getFile`方法返回与`getPath`相同的内容，再加上`getQuery`的值（如果有的话）。

**`getRef`**

返回 URL 的引用组件。

* * *

**注意：**

请记住，并非所有 URL 地址都包含这些组件。URL 类提供这些方法是因为 HTTP URL 包含这些组件，可能是最常用的 URL。URL 类在某种程度上是以 HTTP 为中心的。

* * *

您可以使用这些`get*XXX*`方法获取有关 URL 的信息，而不管您使用哪种构造函数创建 URL 对象。

URL 类以及这些访问器方法使您永远不必再解析 URL！只需给定任何 URL 的字符串规范，创建一个新的 URL 对象，并调用所需信息的任何访问器方法即可。这个小例子程序从字符串规范创建一个 URL，然后使用 URL 对象的访问器方法解析 URL：

```java
import java.net.*;
import java.io.*;

public class ParseURL {
    public static void main(String[] args) throws Exception {

        URL aURL = new URL("http://example.com:80/docs/books/tutorial"
                           + "/index.html?name=networking#DOWNLOADING");

        System.out.println("protocol = " + aURL.getProtocol());
        System.out.println("authority = " + aURL.getAuthority());
        System.out.println("host = " + aURL.getHost());
        System.out.println("port = " + aURL.getPort());
        System.out.println("path = " + aURL.getPath());
        System.out.println("query = " + aURL.getQuery());
        System.out.println("filename = " + aURL.getFile());
        System.out.println("ref = " + aURL.getRef());
    }
}

```

下面是程序显示的输出：

```java
protocol = http
authority = example.com:80
host = example.com
port = 80
path = /docs/books/tutorial/index.html
query = name=networking
filename = /docs/books/tutorial/index.html?name=networking
ref = DOWNLOADING

```
