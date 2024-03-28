# 为扩展设置权限

> 原文：[`docs.oracle.com/javase/tutorial/ext/security/policy.html`](https://docs.oracle.com/javase/tutorial/ext/security/policy.html)

如果有安全管理器生效，那么必须满足以下条件，以使任何软件，包括扩展软件，能够执行安全敏感操作：

+   扩展中的安全敏感代码必须包装在`PrivilegedAction`对象中。

+   安全管理器实施的安全策略必须授予扩展适当的权限。默认情况下，安装的扩展被授予所有安全权限，就好像它们是核心平台 API 的一部分。安全策略授予的权限仅适用于包装在`PrivilegedAction`实例中的代码。

让我们更详细地看一下这些条件，附带一些示例。

## 使用`PrivilegedAction`类

假设你想要修改上一课程示例中扩展示例中的`RectangleArea`类，将矩形面积写入文件而不是标准输出。然而，向文件写入是一个涉及安全的操作，因此如果你的软件将在安全管理器下运行，你需要将你的代码标记为特权代码。你需要执行两个步骤来实现这一点：

1.  你需要将执行安全敏感操作的代码放在类型为`java.security.PrivilegedAction`的对象的`run`方法中。

1.  你必须将该`PrivilegedAction`对象作为参数传递给`java.security.AccessController`的`doPrivileged`方法。

如果我们将这些准则应用于`RectangleArea`类，我们的类定义将如下所示：

```java
import java.io.*;
import java.security.*;

public final class RectangleArea {
    public static void
    writeArea(final java.awt.Rectangle r) {
        AccessController.
          doPrivileged(new PrivilegedAction() {
            public Object run() {
                try { 
                    int area = r.width * r.height;
                    String userHome = System.getProperty("user.home");
                    FileWriter fw = new FileWriter( userHome + File.separator
                        + "test" + File.separator + "area.txt");
                    fw.write("The rectangle's area is " + area);
                    fw.flush();
                    fw.close();
                } catch(IOException ioe) {
                    System.err.println(ioe);
                }
                return null;
            }
        });
    }
}

```

这个类中的唯一方法`writeArea`计算矩形的面积，并将面积写入名为`area.txt`的文件中，该文件位于用户主目录下的`test`目录中。

处理输出文件的安全敏感语句必须放在新的`PrivilegedAction`实例的`run`方法中。（注意，`run`要求返回一个`Object`实例。返回的对象可以是`null`。）然后将新的`PrivilegedAction`实例作为参数传递给`AccessController.doPrivileged`的调用。

有关使用`doPrivileged`的更多信息，请参阅[JDK™文档中的特权块 API](https://docs.oracle.com/javase/8/docs/technotes/guides/security/doprivileged.html)。

以这种方式将安全敏感代码包装在`PrivilegedAction`对象中是使扩展能够执行安全敏感操作的第一个要求。第二个要求是：让安全管理器授予特权代码适当的权限。

## 使用安全策略指定权限

运行时生效的安全策略由*策略文件*指定。默认策略由 JRE 软件中的`lib/security/java.policy`文件设置。

策略文件通过*grant*条目为软件分配安全权限。策略文件可以包含任意数量的 grant 条目。默认策略文件为安装的扩展程序具有以下 grant 条目：

```java
grant codeBase "file:${{java.ext.dirs}}/*" {
    permission java.security.AllPermission;
};

```

此条目指定由`file:${{java.ext.dirs}}/*`指定的目录中的文件将被授予名为`java.security.AllPermission`的权限。 （请注意，从 Java 6 开始，`java.ext.dirs`指的是类似`classpath`的目录路径，每个目录都可以容纳安装的扩展。）不难猜到`java.security.AllPermission`授予安装的扩展程序可以授予的所有安全权限。

默认情况下，安装的扩展程序没有安全限制。只要安全敏感代码包含在作为`doPrivileged`调用参数传递的`PrivilegedAction`实例中，扩展软件就可以执行安全敏感操作，就好像没有安装安全管理器一样。

要限制授予扩展程序的权限，您需要修改策略文件。要拒绝所有扩展程序的所有权限，您可以简单地删除上述 grant 条目。

并非所有权限都像默认授予的`java.security.AllPermission`那样全面。删除默认 grant 条目后，您可以为特定权限输入新的 grant 条目，包括：

+   `java.awt.**AWTPermission**`

+   `java.io.**FilePermission**`

+   `java.net.**NetPermission**`

+   `java.util.**PropertyPermission**`

+   `java.lang.reflect.**ReflectPermission**`

+   `java.lang.**RuntimePermission**`

+   `java.security.**SecurityPermission**`

+   `java.io.**SerializablePermission**`

+   `java.net.**SocketPermission**`

[JDK 中的权限](https://docs.oracle.com/javase/8/docs/technotes/guides/security/permissions.html)文档提供了关于每个权限的详细信息。让我们看看使用 RectangleArea 作为扩展所需的权限。

`RectangleArea.writeArea`方法需要两个权限：一个用于确定用户主目录的路径，另一个用于写入文件。假设`RectangleArea`类打包在文件`area.jar`中，您可以通过向策略文件添加以下条目来授予写入权限：

```java
grant codeBase "file:${java.home}/lib/ext/area.jar" {
    permission java.io.PropertyPermission "user.home",
        "read";
    permission java.io.FilePermission
        "${user.home}${/}test${/}*", "write";
};

```

此条目的`codeBase "file:${java.home}/lib/ext/area.jar"`部分保证此条目指定的权限仅适用于`area.jar`。 `java.io.PropertyPermission`允许访问属性。 第一个参数`"user.home"`指定属性的名称，第二个参数`"read"`表示可以读取该属性。（另一个选择是`"write"`。）

`java.io.FilePermission`允许访问文件。第一个参数`"${user.home}${/}test${/}*"`表示`area.jar`被授予访问用户主目录中`test`目录中所有文件的权限。（请注意`${/}`是一个平台无关的文件分隔符。）第二个参数表示被授予的文件访问权限仅限于写入。（第二个参数的其他选择是`"read"`、`"delete"`和`"execute"`。）

## 签署扩展程序

你可以使用策略文件对扩展程序授予的权限施加额外限制，要求它们必须由受信任的实体签名。（有关签署和验证 JAR 文件的审查，请参阅本教程中的签署 JAR 文件课程。）

为了允许在授予权限时与扩展程序或其他软件一起进行签名验证，策略文件必须包含一个*keystore 条目*。密钥库条目指定用于验证的密钥库。密钥库条目的形式为

```java
keystore "*keystore_url*";

```

URL *keystore_url*可以是绝对或相对的。如果是相对的，URL 是相对于策略文件的位置。例如，要使用`keytool`使用的默认密钥库，将此条目添加到`java.policy`中

```java
keystore "file://${user.home}/.keystore";

```

要指示必须签署扩展程序才能被授予安全权限，您可以使用`signedBy`字段。例如，以下条目指示只有当扩展程序`area.jar`由密钥库中的别名为 Robert 和 Rita 的用户签名时，才授予列出的权限：

```java
grant signedBy "Robert,Rita",
    codeBase "file:${java.home}/lib/ext/area.jar" {
        permission java.io.PropertyPermission
            "user.home", "read";
        permission java.io.FilePermission
            "${user.home}${/}test${/}*", "write";
};

```

如果`codeBase`字段被省略，如下所示的"grant"，则权限将授予*任何*由"Robert"或"Rita"签名的软件，包括已安装或下载的扩展程序：

```java
grant signedBy "Robert,Rita" {
    permission java.io.FilePermission "*", "write";  
};

```

有关策略文件格式的更多详细信息，请参阅 JDK 文档中的[安全体系结构规范](https://docs.oracle.com/javase/8/docs/technotes/guides/security/spec/security-spec.doc3.html#20131)第 3.3.1 节。
