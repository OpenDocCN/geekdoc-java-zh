# 与其他小程序通信

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/iac.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/iac.html)

一个 Java 小程序可以通过在父网页中使用 JavaScript 函数与其他 Java 小程序通信。JavaScript 函数通过接收来自一个小程序的消息并调用其他小程序的方法来实现小程序之间的通信。有关 Java 代码与 JavaScript 代码交互的更多信息，请参阅以下主题：

+   从小程序调用 JavaScript 代码

+   从 JavaScript 代码调用小程序方法

您应避免使用以下机制来查找其他小程序并在小程序之间共享数据：

+   避免使用静态变量在小程序之间共享数据。

+   不要使用[`AppletContext`](https://docs.oracle.com/javase/8/docs/api/java/applet/AppletContext.html)类的`getApplet`和`getApplets`方法来查找其他小程序。这些方法只能找到在同一 Java 运行时环境软件实例中运行的小程序。

小程序必须来自服务器上的同一目录，以便彼此通信。

发送器和接收器小程序如下所示。当用户点击按钮增加计数器时，发送器小程序调用 JavaScript 函数向接收器小程序发送请求。接收器小程序在接收请求后增加一个计数器变量并显示变量的值。

//&lt;![CDATA[ function sendMsgToIncrementCounter() { var myReceiver = document.getElementById("receiver"); myReceiver.incrementCounter(); } //]]&gt;

发送器小程序

//&lt;![CDATA[ var attributes = { code:'Sender.class', archive:'examples/dist/applet_SenderReceiver/applet_SenderReceiver.jar', width:300, height:50} ; var parameters = { permissions:'sandbox' }; deployJava.runApplet(attributes, parameters, '1.6'); //]]&gt;

接收器小程序

//&lt;![CDATA[ var attributes = { id:'receiver', code:'Receiver.class', archive:'examples/dist/applet_SenderReceiver/applet_SenderReceiver.jar', width:300, height:50} ; var parameters = { permissions:'sandbox' }; deployJava.runApplet(attributes, parameters, '1.6'); //]]&gt;

* * *

**注意：** 如果您看不到小程序运行，请至少安装[Java SE Development Kit (JDK) 6 update 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

* * *

**注意：** 如果您看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

要启用与另一个小程序的通信，获取 `netscape.javascript.JSObject` 类的实例的引用。使用此实例调用 JavaScript 函数。`Sender` 小程序使用 `netscape.javascript.JSObject` 类的实例来调用名为 `sendMsgToIncrementCounter` 的 JavaScript 函数。

```java
try {
    JSObject window = JSObject.getWindow(this);
    window.eval("sendMsgToIncrementCounter()");
} catch (JSException jse) {
    // ...
}

```

* * *

**注意：** 要编译具有对 `netscape.javascript` 包中类的引用的 Java 代码，请在类路径中包含 `<your JDK path>/jre/lib/plugin.jar`。在运行时，Java 插件软件会自动使这些类对小程序可用。

* * *

编写 JavaScript 函数，该函数将接收来自一个小程序的请求，并调用网页上另一个小程序的方法。`sendMsgToIncrementCounter` JavaScript 函数调用接收器小程序的 `incrementCounter` 方法。

```java
<script>
    function sendMsgToIncrementCounter() {
        var myReceiver = document.getElementById("receiver");
        myReceiver.incrementCounter();
    } 
<script>

```

请注意，JavaScript 代码使用名称 `receiver` 来获取网页上接收器小程序的引用。此名称应与部署接收器小程序时指定的 `id` 属性的值相同。

下面显示了 `Receiver` 小程序的 `incrementCounter` 方法。

```java
public void incrementCounter() {
    ctr++;
    String text = " Current Value Of Counter: "
        + (new Integer(ctr)).toString();
    ctrLbl.setText(text);
}

```

部署小程序在网页上，如下面的代码片段所示。您可以在 ``AppletPage.html`` 中查看发送器和接收器小程序以及相关的 JavaScript 代码。

```java
<!-- Sender Applet -->
<script src="https://www.java.com/js/deployJava.js"></script>
<script> 
    var attributes = { code:'Sender.class',
        archive:'examples/dist/applet_SenderReceiver/applet_SenderReceiver.jar',
        width:300, height:50} ;
    var parameters = { permissions:'sandbox' };
    deployJava.runApplet(attributes, parameters, '1.6');
</script>

<!-- Receiver Applet -->
<script> 
    var attributes = { id:'receiver', code:'Receiver.class',
        archive:'examples/dist/applet_SenderReceiver/applet_SenderReceiver.jar',
        width:300, height:50} ;
    var parameters = { permissions:'sandbox' };
    deployJava.runApplet(attributes, parameters, '1.6');
</script>

```

下载源代码 以进一步进行实验的 *发送器接收器小程序* 示例。
