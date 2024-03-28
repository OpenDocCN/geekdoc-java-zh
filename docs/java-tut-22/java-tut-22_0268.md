# 定义和使用 Applet 参数

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/param.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/param.html)

参数对于 Java applet 就像命令行参数对于应用程序一样重要。它们使用户能够自定义 applet 的操作。通过定义参数，你可以增加 applet 的灵活性，使其在多种情况下工作而无需重新编码和重新编译。

## 指定 Applet 的输入参数

你可以在 applet 的 Java 网络启动协议（JNLP）文件或 `<applet>` 标签的 `<parameter>` 元素中指定 applet 的输入参数。通常最好在 applet 的 JNLP 文件中指定参数，这样即使 applet 部署在多个网页上，参数也可以一致提供。如果 applet 的参数会因网页而异，则应在 `<applet>` 标签的 `<parameter>` 元素中指定参数。

如果你对 JNLP 不熟悉，请查看 Java 网络启动协议 主题获取更多信息。

考虑一个需要三个参数的 applet。`paramStr` 和 `paramInt` 参数在 applet 的 JNLP 文件中指定，`applettakesparams.jnlp`。

```java
<?xml version="1.0" encoding="UTF-8"?>
<jnlp spec="1.0+" codebase="" href="">
    <!-- ... -->
    <applet-desc
         name="Applet Takes Params"
         main-class="AppletTakesParams"
         width="800"
         height="50">
             <param name="paramStr"
                 value="someString"/>
             <param name="paramInt" value="22"/>
     </applet-desc>
     <!-- ... -->
</jnlp>

```

`paramOutsideJNLPFile` 参数在传递给部署工具包脚本的 `runApplet` 函数的 `parameters` 变量中指定在 `AppletPage.html`。

```java
<html>
  <head>
    <title>Applet Takes Params</title>
    <meta http-equiv="Content-Type" content="text/html;
        charset=windows-1252">
  </head>
  <body>
    <h1>Applet Takes Params</h1>

    <script
      src="https://www.java.com/js/deployJava.js"></script>
    <script>
        var attributes = { code:'AppletTakesParams.class',
            archive:'applet_AppletWithParameters.jar',
            width:800, height:50 };
        var parameters = {jnlp_href: 'applettakesparams.jnlp',
            paramOutsideJNLPFile: 'fooOutsideJNLP' };
        deployJava.runApplet(attributes, parameters, '1.7');
    </script>

  </body>
</html>

```

查看 部署 Applet 获取有关 `runApplet` 函数的更多信息。

## 检索 Applet 的输入参数

你可以使用 `Applet` 类的 [`getParameter`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html#getParameter-java.lang.String-) 方法来检索 applet 的输入参数。`AppletTakesParams.java` applet 检索并显示其所有输入参数（`paramStr`，`paramInt` 和 `paramOutsideJNLPFile`）。

```java

import javax.swing.JApplet;
import javax.swing.SwingUtilities;
import javax.swing.JLabel;

public class AppletTakesParams extends JApplet {
    public void init() {
        final String  inputStr = getParameter("paramStr");        
        final int inputInt = Integer.parseInt(getParameter("paramInt"));
        final String inputOutsideJNLPFile = getParameter("paramOutsideJNLPFile");

        try {
            SwingUtilities.invokeAndWait(new Runnable() {
                public void run() {
                    createGUI(inputStr, inputInt, inputOutsideJNLPFile);
                }
            });
        } catch (Exception e) {
            System.err.println("createGUI didn't successfully complete");
        }
    }
    private void createGUI(String inputStr, int inputInt, String inputOutsideJNLPFile) {
        String text = "Applet's parameters are -- inputStr: " + inputStr +
                ",   inputInt: " + inputInt +
                ",   paramOutsideJNLPFile: " + inputOutsideJNLPFile;
        JLabel lbl = new JLabel(text);
        add(lbl);
    }
}

```

下面显示了 `AppletTakesParams` applet。

//&lt;![CDATA[ var attributes = { code:'AppletTakesParams.class', archive:'examples/dist/applet_AppletWithParameters/applet_AppletWithParameters.jar', width:800, height:50} ; var parameters = { jnlp_href: 'examples/dist/applet_AppletWithParameters/applettakesparams.jnlp', paramOutsideJNLPFile: 'fooOutsideJNLP' }; deployJava.runApplet(attributes, parameters, '1.6'); //]]&gt;

* * *

**注意：** 如果你看不到 applet 运行，你需要安装至少 [Java SE Development Kit (JDK) 6 update 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 版本。

* * *

* * *

**注意：** 如果你看不到示例运行，可能需要在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

下载源代码 以进一步进行实验。
