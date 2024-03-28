# 在扩展中封存包

> 原文：[`docs.oracle.com/javase/tutorial/ext/security/sealing.html`](https://docs.oracle.com/javase/tutorial/ext/security/sealing.html)

您可以选择性地*封存*扩展 JAR 文件中的包作为额外的安全措施。如果一个包被封存，那么该包中定义的所有类必须来自一个单独的 JAR 文件。

没有封存，一个“敌对”的程序可以创建一个类并将其定义为您的扩展包的成员。然后，敌对软件将自由访问您的扩展包的包保护成员。

在扩展中封存包与封存任何 JAR 打包的类没有区别。要封存您的扩展包，您必须向包含您的扩展的 JAR 文件的清单中添加`Sealed`头部。您可以通过将`Sealed`头部与包的`Name`头部关联来封存单个包。在存档中未与单个包关联的`Sealed`头部表示所有包都被封存。这样的“全局”`Sealed`头部将被与单个包关联的任何`Sealed`头部覆盖。与`Sealed`头部关联的值要么是`true`，要么是`false`。

## 例子

让我们看几个示例清单文件。在这些示例中，假设 JAR 文件包含这些包：

```java
com/myCompany/package_1/
com/myCompany/package_2/
com/myCompany/package_3/
com/myCompany/package_4/

```

假设您想要封存所有包。您可以通过简单地向清单中添加一个存档级别的`Sealed`头部来实现：

```java
Manifest-Version: 1.0
Sealed: true

```

任何具有这个清单的 JAR 文件中的所有包都将被封存。

如果您只想封存`com.myCompany.package_3`，您可以通过这个清单来实现：

```java
Manifest-Version: 1.0

Name: com/myCompany/package_3/
Sealed: true

```

在这个例子中，唯一的`Sealed`头部是与包`com.myCompany.package_3`的`Name`头部相关联的，因此只有该包被封存。（`Sealed`头部与`Name`头部相关联，因为它们之间没有空行。）

最后一个例子，假设您想要封存所有包*除了*`com.myCompany.package_2`。您可以通过这样的清单来实现：

```java
Manifest-Version: 1.0
Sealed: true

Name: com/myCompany/package_2/
Sealed: false

```

在这个例子中，存档级别的`Sealed: true`头部表示 JAR 文件中的所有包都将被封存。然而，清单中还有一个与包`com.myCompany.package_2`相关联的`Sealed: false`头部，该头部覆盖了该包的存档级别封存。因此，这个清单将导致所有包都被封存，除了`com.myCompany.package_2`。
