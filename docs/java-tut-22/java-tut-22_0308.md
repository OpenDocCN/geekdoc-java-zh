# 更改启动按钮

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/changeLaunchButtonOfJWS.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/changeLaunchButtonOfJWS.html)

如果您不喜欢默认的启动按钮![启动按钮](img/4707a69a17729d71c56b2bdbbb4cc61c.png)，或者您有另一幅标准化的图像，您可以更改您的 Java Web Start 应用程序的启动按钮图像。

使用`deployJava.launchButtonPNG`变量指向启动按钮图像的位置。

**变量:** `deployJava.launchButtonPNG`

**用法:** 提供替代图像 URL

在这个例子中，记事本应用程序的启动按钮现在是杜克挥手的图像。

```java

<script src="https://www.java.com/js/deployJava.js"></script>
<script>
    deployJava.launchButtonPNG='https://docs.oracle.com/javase/tutorial/images/DukeWave.gif';
    var url = "https://docs.oracle.com/javase/tutorialJWS/samples/deployment/NotepadJWSProject/Notepad.jnlp";
    deployJava.createWebStartLaunchButton(url, '1.6.0');
</script>

```

记事本应用程序的新启动按钮（杜克挥手）如下。点击杜克的图像启动记事本应用程序。

//&lt;![CDATA[ deployJava.launchButtonPNG='https://docs.oracle.com/javase/tutorial/images/DukeWave.gif'; var url = 'https://docs.oracle.com/javase/tutorialJWS/samples/deployment/NotepadJWSProject/Notepad.jnlp'; deployJava.createWebStartLaunchButton(url, '1.6.0'); //]]&gt;

* * *

**注意:** 如果您看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *
