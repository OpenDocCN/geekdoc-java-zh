# 下载扩展

> 原文：[`docs.oracle.com/javase/tutorial/ext/basics/download.html`](https://docs.oracle.com/javase/tutorial/ext/basics/download.html)

下载扩展是 JAR 文件中的一组类（和相关资源）。JAR 文件的清单可以包含引用一个或多个下载扩展的头部。这些扩展可以通过以下两种方式引用：

+   通过`Class-Path`头部

+   通过`Extension-List`头部

请注意，清单中最多只允许一个。通过`Class-Path`头部指示的下载扩展仅在下载它们的应用程序（如 Web 浏览器）的生命周期内下载。它们的优点是客户端上没有安装任何内容；缺点是每次需要时都会下载它们。通过`Extension-List`头部下载的下载扩展将安装到下载它们的 JRE 的`/lib/ext`目录中。它们的优点是第一次需要时下载，随后可以在不下载的情况下使用。但是，正如本教程后面所示，它们部署起来更加复杂。

由于使用`Class-Path`头部的下载扩展更简单，让我们先考虑它们。例如假设`a.jar`和`b.jar`是同一目录中的两个 JAR 文件，并且`a.jar`的清单包含了这个头部：

```java
Class-Path: b.jar

```

那么`b.jar`中的类将作为`a.jar`中的类的扩展类。`a.jar`中的类可以调用`b.jar`中的类，而无需将`b.jar`中的类命名在类路径中。`a.jar`本身可能是扩展，也可能不是。如果`b.jar`不在与`a.jar`相同的目录中，那么`Class-Path`头部的值应设置为`b.jar`的相对路径名。

扮演下载扩展角色的类没有任何特殊之处。它们之所以被视为扩展，仅仅是因为它们被某个其他 JAR 文件的清单引用。

为了更好地理解下载扩展的工作原理，让我们创建一个并投入使用。

## 一个示例

假设你想要创建一个小程序，其中使用了前一节中的`RectangleArea`类：

```java
public final class RectangleArea {  
    public static int area(java.awt.Rectangle r) {
        return r.width * r.height;
    }
}

```

在前一节中，你将`RectangleArea`类放入 JRE 的`lib/ext`目录中，将其转换为已安装扩展。通过将其转换为已安装扩展，任何应用程序都可以使用`RectangleArea`类，就好像它是 Java 平台的一部分。

如果你想要在小程序中使用`RectangleArea`类，情况会有些不同。例如，假设你有一个名为`AreaApplet`的小程序，其中使用了`RectangleArea`类：

```java
import java.applet.Applet;
import java.awt.*;

public class AreaApplet extends Applet {
    Rectangle r;

    public void init() {    
        int width = 10;
        int height = 5;

        r = new Rectangle(width, height);
    }

    public void paint(Graphics g) {
        g.drawString("The rectangle's area is " 
                      + RectangleArea.area(r), 10, 10);
    }
}

```

此小程序实例化一个 10 `x` 5 的矩形，然后使用`RectangleArea.area`方法显示矩形的面积。

然而，你不能假设每个下载并使用你的小程序的人都会在他们的系统上有`RectangleArea`类可用，作为已安装的扩展或其他方式。解决这个问题的一种方法是从服务器端提供`RectangleArea`类，并且你可以通过将其作为下载扩展来实现。

要了解如何做到这一点，让我们假设你已经将`AreaApplet`捆绑在名为`AreaApplet.jar`的 JAR 文件中，并且类`RectangleArea`捆绑在`RectangleArea.jar`中。为了使`RectangleArea.jar`被视为下载扩展，`RectangleArea.jar`必须在`AreaApplet.jar`的清单中的`Class-Path`头中列出。例如，`AreaApplet.jar`的清单可能如下所示：

```java
Manifest-Version: 1.0
Class-Path: RectangleArea.jar

```

这个清单中`Class-Path`头的值是`RectangleArea.jar`，没有指定路径，表示`RectangleArea.jar`位于与小程序的 JAR 文件相同的目录中。

## 关于`Class-Path`头的更多信息

如果一个小程序或应用程序使用多个扩展，你可以在清单中列出多个 URL。例如，以下是一个有效的头部：

```java
Class-Path: area.jar servlet.jar images/

```

在`Class-Path`头中，列出的任何不以'`/`'结尾的 URL 都被假定为 JAR 文件。以'`/`'结尾的 URL 表示目录。在上面的例子中，`images/`可能是一个包含小程序或应用程序所需资源的目录。

请注意，清单文件中只允许一个`Class-Path`头，并且清单中的每一行不能超过 72 个字符。如果需要指定的类路径条目超过一行的空间，可以将它们延伸到后续的续行上。每个续行都以两个空格开头。例如：

```java
Class-Path: area.jar servlet.jar monitor.jar datasource.jar
  provider.jar gui.jar

```

未来的版本可能会取消每个标题只能有一个实例的限制，以及将行限制为仅有 72 个字符。

下载扩展可以“串联”，意味着一个下载扩展的清单可以有一个引用第二个扩展的`Class-Path`头，第二个扩展可以引用第三个扩展，依此类推。

## 安装下载扩展

在上面的例子中，小程序下载的扩展仅在加载小程序的浏览器仍在运行时可用。然而，如果在小程序和扩展的清单中包含了额外的信息，小程序可以触发扩展的安装。

由于这种机制扩展了平台的核心 API，其使用应谨慎。它很少适用于仅由单个或少量应用程序使用的接口。所有可见的符号应遵循反向域名和类层次结构约定。

基本要求是小程序和它使用的扩展在它们的清单中提供版本信息，并且它们被签名。版本信息允许 Java 插件确保扩展代码具有小程序期望的版本。例如，`AreaApplet`可以在其清单中指定一个`areatest`扩展：

```java
Manifest-Version: 1.0
Extension-List: areatest
areatest-Extension-Name: area
areatest-Specification-Version: 1.1
areatest-Implementation-Version: 1.1.2
areatest-Implementation-Vendor-Id: com.example
areatest-Implementation-URL: http://www.example.com/test/area.jar

```

`area.jar`中的清单将提供相应的信息：

```java
Manifest-Version: 1.0
Extension-Name: area
Specification-Vendor: Example Tech, Inc
Specification-Version: 1.1
Implementation-Vendor-Id: com.example
Implementation-Vendor: Example Tech, Inc
Implementation-Version: 1.1.2

```

应用程序和扩展程序都必须由相同的签名者签名。对 jar 文件进行签名会直接修改它们，在清单文件中提供更多信息。签名有助于确保只有可信任的代码被安装。签署 jar 文件的简单方法是首先创建一个密钥库，然后使用该密钥库保存用于应用程序和扩展程序的证书。例如：

```java
keytool -genkey -dname "cn=Fred" -alias test  -validity 180

```

您将被要求输入密钥库和密钥密码。生成密钥后，jar 文件可以被签名：

```java
jarsigner AreaApplet.jar test
jarsigner area.jar test

```

您将被要求输入密钥库和密钥密码。有关`keytool`、`jarsigner`和其他安全工具的更多信息，请参阅[Java 2 平台安全工具概述](https://docs.oracle.com/javase/8/docs/technotes/guides/security/SecurityToolsSummary.html)。

这里是`AreaDemo.html`，它加载应用程序并导致扩展程序代码被下载并安装：

```java
<html>
<body>
  <applet code="AreaApplet.class" archive="AreaApplet.jar"/>
</body>
</html>

```

当页面首次加载时，用户会被告知该应用程序需要安装扩展程序。随后的对话框会通知用户有关已签名的应用程序。接受两者会将扩展程序安装在 JRE 的`lib/ext`文件夹中并运行应用程序。

重新启动 Web 浏览器并加载相同的网页后，只会显示有关应用程序签名者的对话框，因为`area.jar`已经安装。如果在不同的 Web 浏览器中打开`AreaDemo.html`（假设两个浏览器都使用相同的 JRE），情况也是如此。
