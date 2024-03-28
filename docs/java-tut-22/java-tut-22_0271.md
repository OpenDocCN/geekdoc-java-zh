# 从小程序调用 JavaScript 代码

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/invokingJavaScriptFromApplet.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/invokingJavaScriptFromApplet.html)

Java 小程序可以调用与小程序在同一网页中的 JavaScript 函数。[LiveConnect 规范](http://www.oracle.com/technetwork/java/javase/plugin2-142482.html#LIVECONNECT)描述了 JavaScript 代码如何与 Java 代码通信的详细信息。

`netscape.javascript.JSObject`类使 Java 小程序能够检索对 JavaScript 对象的引用并与网页交互。接下来描述的数据摘要小程序调用 JavaScript 代码从网页中检索信息，并将数据摘要写回网页。

假设您有一个带有几个 JavaScript 函数的网页。示例``AppletPage.html``具有用于检索年龄、地址和电话号码的 JavaScript 函数。还有一个名为`userName`的变量，一开始没有值。

```java
<head>
<title>Data Summary Applet Page - Java to JavaScript LiveConnect</title>
<meta http-equiv="Content-Type" content="text/html; charset=windows-1252"/>
<script language="javascript">
    var userName = "";

    // returns number
    function getAge() { 
        return 25;
    }
    // returns an object
    function address() { 
        this.street = "1 Example Lane";
        this.city = "Santa Clara";
        this.state = "CA";
    }
    // returns an array
    function getPhoneNums() { 
        return ["408-555-0100", "408-555-0102"];
    } 
    function writeSummary(summary) {
        summaryElem =
            document.getElementById("summary");
        summaryElem.innerHTML = summary;
    }
    </script>

    <!-- ... -->      
</head>
<body>
    <script src =
      "https://www.java.com/js/deployJava.js"></script>
    <script> 
        <!-- ... -->
        deployJava.runApplet(attributes, parameters, '1.6'); 
    </script>          
    <!-- ... -->
    <p id="summary"/>  // this HTML element contains
                             // the summary 
    <!-- ... -->
</body>

```

接下来，考虑一个名为`DataSummaryApplet`的小程序类。`DataSummaryApplet`类执行以下操作。

+   调用`JSObject`的`setMember`方法将`userName`变量设置为"John Doe"。

+   检索年龄、地址和电话号码，并构建包含这些数据摘要的字符串。

+   调用`writeSummary` JavaScript 函数将摘要写回网页。

该小程序首先需要按以下方式检索对`JSObject`的引用：

```java
...
JSObject window = JSObject.getWindow(this);
...

```

将前述语句放入 try...catch...块中以处理`netscape.javascript.JSException`。

现在，小程序已经有了对`JSObject`的引用，它可以通过`JSObject`的`eval`和`call`方法调用相关的 JavaScript 函数。

```java

package javatojs;

import java.applet.Applet;
import netscape.javascript.*; // add plugin.jar to classpath during compilation

public class DataSummaryApplet extends Applet {
    public void start() {
        try {
            JSObject window = JSObject.getWindow(this);

            String userName = "John Doe";

            // set JavaScript variable
            window.setMember("userName", userName);

            // invoke JavaScript function
            Number age = (Number) window.eval("getAge()");

            // get a JavaScript object and retrieve its contents
            JSObject address = (JSObject) window.eval("new address();");
            String addressStr = (String) address.getMember("street") + ", " +
                    (String) address.getMember("city") + ", " +
                    (String) address.getMember("state");

            // get an array from JavaScript and retrieve its contents
            JSObject phoneNums = (JSObject) window.eval("getPhoneNums()");
            String phoneNumStr = (String) phoneNums.getSlot(0) + ", " +
                    (String) phoneNums.getSlot(1);

            // dynamically change HTML in page; write data summary
            String summary = userName + " : " + age + " : " +
                    addressStr + " : " + phoneNumStr;
            window.call("writeSummary", new Object[] {summary})   ;
        } catch (JSException jse) {
            jse.printStackTrace();
        }
    }
}

```

要编译具有对`netscape.javascript`包中类的引用的 Java 代码，请在类路径中包含`<您的 JDK 路径>/jre/lib/plugin.jar`。在运行时，Java 插件软件会自动使这些类对小程序可用。

数据摘要小程序在网页上显示以下结果：

```java
Result of applet's Java calls to JavaScript on this page

John Doe : 25 : 1 Example Lane, Santa Clara, CA : 408-555-0100, 408-555-0102

```

在浏览器中打开``AppletPage.html``以查看数据摘要小程序。

* * *

**注意：** 如果您看不到小程序运行，请安装至少[Java SE Development Kit (JDK) 6 update 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

* * *

**注意：** 如果您看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

下载源代码以进一步实验*从小程序调用 JavaScript 代码*示例。
