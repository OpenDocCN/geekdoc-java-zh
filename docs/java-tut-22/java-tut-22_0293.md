# 设置可信参数和安全属性

> 原文：[`docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/settingArgsProperties.html`](https://docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/settingArgsProperties.html)

你可以在富互联网应用程序（RIA）的 Java 网络启动协议（JNLP）文件中设置特定的 Java 虚拟机参数和安全属性。对于小程序，你还可以在`<applet>`标签的`java_arguments`参数中设置参数。虽然有一组预定义的安全属性，但你也可以通过在属性名称前加上"`jnlp.`"或"`javaws.`"来定义新的安全属性。可以通过使用`System.getProperty`方法在你的 RIA 中检索属性。

考虑属性和参数演示小程序。以下 Java 虚拟机参数和属性在小程序的 JNLP 文件`appletpropsargs.jnlp`中设置。

+   `-Xmx` – 一个安全参数，设置为"256M"

+   `sun.java2d.noddraw` – 一个预定义的安全属性，设置为"true"

+   `jnlp.myProperty` – 一个用户定义的安全属性，设置为"a user-defined property"

```java
<?xml version="1.0" encoding="UTF-8"?>
<jnlp spec="1.0+" codebase="" href="">
    <information>
        <title>Properties and Arguments Demo Applet</title>
        <vendor>Dynamic Team</vendor>
    </information>
    <resources>
        <!-- Application Resources -->
        <j2se version="1.6+"
              href="http://java.sun.com/products/autodl/j2se"
              <!-- secure java vm argument -->
              java-vm-args="-Xmx256M"/>
        <jar href="applet_PropertiesAndVMArgs.jar"
            main="true" />
            <!-- secure properties -->
        <property name="sun.java2d.noddraw"
            value="true"/>
        <property name="jnlp.myProperty"
            value="a user-defined property"/>
    </resources>
    <applet-desc 
         name="Properties and Arguments Demo Applet"
         main-class="PropertiesArgsDemoApplet"
         width="800"
         height="50">             
     </applet-desc>
     <update check="background"/>
</jnlp>

```

`PropertiesArgsDemoApplet`类使用`System.getProperty`方法检索在 JNLP 文件中设置的`java.version`属性和其他属性。`PropertiesArgsDemoApplet`类还显示这些属性。

```java

import javax.swing.JApplet;
import javax.swing.SwingUtilities;
import javax.swing.JLabel;

public class PropertiesArgsDemoApplet extends JApplet {
    public void init() {
        final String javaVersion = System.getProperty("java.version");
        final String  swing2dNoDrawProperty = System.getProperty("sun.java2d.noddraw");
        final String  jnlpMyProperty = System.getProperty("jnlp.myProperty");        

        try {
            SwingUtilities.invokeAndWait(new Runnable() {
                public void run() {
                    createGUI(javaVersion, swing2dNoDrawProperty, jnlpMyProperty);
                }
            });
        } catch (Exception e) {
            System.err.println("createGUI didn't successfully complete");
        }
    }
    private void createGUI(String javaVersion, String swing2dNoDrawProperty, String jnlpMyProperty) {
        String text = "Properties: java.version = " + javaVersion + 
                ",  sun.java2d.noddraw = " + swing2dNoDrawProperty +
                ",   jnlp.myProperty = " + jnlpMyProperty;
        JLabel lbl = new JLabel(text);
        add(lbl);
    }
}

```

接下来显示属性和参数演示小程序。你也可以在`AppletPage.html`中看到小程序运行。

//&lt;![CDATA[ var attributes = { code:'PropertiesArgsDemoApplet.class', archive:'examples/dist/applet_PropertiesAndVMArgs/applet_PropertiesAndVMArgs.jar', width:800, height:50} ; var parameters = { jnlp_href: 'examples/dist/applet_PropertiesAndVMArgs/appletpropsargs.jnlp' }; deployJava.runApplet(attributes, parameters, '1.6'); //]]&gt;

* * *

**注意：** 如果你看不到小程序运行，你需要安装至少[Java SE Development Kit (JDK) 6 update 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

* * *

**注意：** 如果你看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

下载源代码以进一步实验*属性和参数演示小程序*示例。

查看系统属性以获取可以被 RIA 访问的完整系统属性集。
