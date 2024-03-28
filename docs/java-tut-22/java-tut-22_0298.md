# 访问 Cookies

> 原文：[`docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/accessingCookies.html`](https://docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/accessingCookies.html)

您可以在富互联网应用程序（RIA）中设置和检索 Cookies。Cookies 可以增强您的 RIA 的功能。例如，考虑这样的情况：您在各种网页上有小程序。一个网页上的小程序不能直接访问或共享另一个网页上的小程序的信息。在这种情况下，Cookies 提供了小程序之间的重要连接，并帮助一个小程序将信息传递给另一个不同网页上的小程序。Java Web Start 应用程序也可以使用 Cookies 在客户端存储信息。

Cookie 小程序示例有一个`CookieAccessor`类，用于检索和设置 Cookies。

## 检索 Cookies

以下代码片段显示了`CookieAccessor`类的`getCookieUsingCookieHandler`方法：

```java
public void getCookieUsingCookieHandler() { 
    try {       
        // Instantiate CookieManager;
        // make sure to set CookiePolicy
        CookieManager manager = new CookieManager();
        manager.setCookiePolicy(CookiePolicy.ACCEPT_ALL);
        CookieHandler.setDefault(manager);

        // get content from URLConnection;
        // cookies are set by web site
        URL url = new URL("http://host.example.com");
        URLConnection connection = url.openConnection();
        connection.getContent();

        // get cookies from underlying
        // CookieStore
        CookieStore cookieJar =  manager.getCookieStore();
        List <HttpCookie> cookies =
            cookieJar.getCookies();
        for (HttpCookie cookie: cookies) {
          System.out.println("CookieHandler retrieved cookie: " + cookie);
        }
    } catch(Exception e) {
        System.out.println("Unable to get cookie using CookieHandler");
        e.printStackTrace();
    }
}  

```

[`CookieManager`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieManager.html)类是管理 Cookie 的主要入口点。创建`CookieManager`类的实例，并设置其[`CookiePolicy`](https://docs.oracle.com/javase/8/docs/api/java/net/CookiePolicy.html)。将此`CookieManager`类的实例设置为默认的[`CookieHandler`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieHandler.html)。

打开到您选择的网站的[`URLConnection`](https://docs.oracle.com/javase/8/docs/api/java/net/URLConnection.html)。

接下来，使用`getCookies`方法从底层的[`CookieStore`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieStore.html)中检索 Cookies。

## 设置 Cookies

以下代码片段显示了`CookieAccessor`类的`setCookieUsingCookieHandler`方法：

```java
public void setCookieUsingCookieHandler() {
    try {
        // instantiate CookieManager
        CookieManager manager = new CookieManager();
        CookieHandler.setDefault(manager);
        CookieStore cookieJar =  manager.getCookieStore();

        // create cookie
        HttpCookie cookie = new HttpCookie("UserName", "John Doe");

        // add cookie to CookieStore for a
        // particular URL
        URL url = new URL("http://host.example.com");
        cookieJar.add(url.toURI(), cookie);
        System.out.println("Added cookie using cookie handler");
    } catch(Exception e) {
        System.out.println("Unable to set cookie using CookieHandler");
        e.printStackTrace();
    }
}

```

如检索 Cookies 所示，[`CookieManager`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieManager.html)类是管理 Cookie 的主要入口点。创建`CookieManager`类的实例，并将该实例设置为默认的[`CookieHandler`](https://docs.oracle.com/javase/8/docs/api/java/net/CookieHandler.html)。

使用所需信息创建[`HttpCookie`](https://docs.oracle.com/javase/8/docs/api/java/net/HttpCookie.html)。在我们的示例中，我们创建了一个将`UserName`设置为`John Doe`的新`HttpCookie`。

接下来，将 Cookie 添加到底层的 Cookie 存储中。

## 运行 Cookie 小程序示例

要访问 Cookies，您必须对您的 RIA JAR 文件进行签名，并请求在安全沙箱之外运行的权限。请参阅[`jarsigner`](https://docs.oracle.com/javase/8/docs/technotes/tools/index.html#security)工具的文档，了解如何对 JAR 文件进行签名。有关请求权限的信息，请参阅富互联网应用程序中的安全性。

下载源代码以进一步实验*Cookie Applet*示例。
