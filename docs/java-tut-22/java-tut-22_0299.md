# 丰富互联网应用程序中的安全性

> 原文：[`docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/security.html`](https://docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/security.html)

丰富互联网应用程序（RIAs）背后的安全模型旨在保护用户免受恶意互联网应用程序的侵害。本主题讨论了适用于小程序和 Java Web Start 应用程序的安全方面。有关更多信息，请参阅以下主题：

+   小程序的能力和限制

+   Java Web Start 和安全性

RIAs 可以限制在 Java 安全沙箱中运行，或请求权限访问安全沙箱之外的资源。第一次启动 RIA 时，用户将被提示是否允许运行。显示的对话框提供有关签名者证书的信息，并指示 RIA 是否请求在安全沙箱之外运行的权限。用户随后可以做出知情决定是否运行应用程序。

请遵循以下准则来帮助保护您的 RIAs。

+   使用来自认可的证书颁发机构的证书对 RIA 的 JAR 文件进行签名。有关更多信息，请参阅 签署和验证 JAR 文件 主题。

+   如果 RIA 需要访问安全沙箱之外的资源，请在 RIA 的 JNLP 文件中指定 `all-permissions` 元素。否则，让 RIA 默认在安全沙箱中运行。以下代码片段显示了 RIA 的 JNLP 文件中的 `all-permissions` 元素。

    ```java
    <security>
       <all-permissions/>
    </security>

    ```

    如果使用 applet 标签，请参阅 使用 applet 标签部署 以获取有关设置权限级别的信息。

+   一个 JNLP 文件只能包含由相同证书签名的 JAR 文件。如果您有使用不同证书签名的 JAR 文件，请在单独的 JNLP 文件中指定它们。在 RIA 的主 JNLP 文件中，使用 `component-desc` 元素来包含其他 JNLP 文件作为组件扩展。有关详细信息，请参阅 JNLP 文件的结构。

+   对于 RIAs 的安全模型不允许来自网页的 JavaScript 代码调用签名 JAR 文件中的安全敏感代码，除非您明确启用此功能。在签名的 JAR 文件中，将您希望 JavaScript 代码能够调用的代码部分包装在 [`AccessController.doPrivileged`](https://docs.oracle.com/javase/8/docs/api/java/security/AccessController.html) 块中。这样，当执行 `doPrivileged` 代码块中的代码时，JavaScript 代码可以以提升的权限运行。

+   尽量避免在 RIA 中混合特权和沙箱组件，因为它们可能会引发有关混合代码的安全警告。有关更多信息，请参阅 [混合特权代码和沙箱代码](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/mixed_code.html)。

+   在 JAR 文件清单中包含`Permissions`和`Codebase`属性，以确保您的 RIA 请求仅限于您指定的权限，并且该 RIA 是从正确的位置访问的。查看[用于安全性的 JAR 文件清单属性](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html)获取更多信息。

+   JAR 文件清单属性使您能够限制对您的 RIA 的访问，并有助于确保您的代码不被篡改。查看使用清单属性增强安全性获取所有可用的 JAR 文件清单属性的信息。
