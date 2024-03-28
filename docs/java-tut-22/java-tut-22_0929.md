# 连接到 URL

> 原文：[`docs.oracle.com/javase/tutorial/networking/urls/connecting.html`](https://docs.oracle.com/javase/tutorial/networking/urls/connecting.html)

成功创建`URL`对象后，您可以调用`URL`对象的`openConnection`方法获取一个`URLConnection`对象，或者其协议特定的子类之一，例如[`java.net.HttpURLConnection`](https://docs.oracle.com/javase/8/docs/api/java/net/HttpURLConnection.html)

您可以使用此`URLConnection`对象在连接之前设置参数和一般请求属性。仅当调用`URLConnection.connect`方法时才会启动与 URL 表示的远程对象的连接。这样做时，您正在初始化您的 Java 程序与网络上的 URL 之间的通信链接。例如，以下代码打开到`example.com`站点的连接：

```java
try {
    URL myURL = new URL("http://example.com/");
    URLConnection myURLConnection = myURL.openConnection();
    myURLConnection.connect();
} 
catch (MalformedURLException e) { 
    // new URL() failed
    // ...
} 
catch (IOException e) {   
    // openConnection() failed
    // ...
}

```

每次调用此 URL 的协议处理程序的`openConnection`方法都会创建一个新的`URLConnection`对象。

您并不总是需要显式调用`connect`方法来启动连接。依赖于连接的操作，如`getInputStream`、`getOutputStream`等，将在必要时隐式执行连接。

现在您已成功连接到您的 URL，您可以使用`URLConnection`对象执行诸如从连接中读取或写入的操作。接下来的部分将向您展示如何操作。
