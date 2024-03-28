# 使用清单属性增强安全性

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/secman.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/secman.html)

以下 JAR 文件清单属性可帮助确保您的 applet 或 Java Web Start 应用程序的安全性。只有`Permissions`属性是必需的。

+   `Permissions`属性用于确保应用程序仅请求在用于调用应用程序的 applet 标记或 JNLP 文件中指定的权限级别。使用此属性可帮助防止有人重新部署使用您的证书签名的应用程序，并以不同的特权级别运行。

    此属性在主 JAR 文件的清单中是必需的。有关更多信息，请参阅 Java 平台标准版部署指南中的[Permissions Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG896)。

+   `Codebase`属性用于确保 JAR 文件的代码库受限于特定域。使用此属性可防止有人出于恶意目的在另一个网站上重新部署您的应用程序。有关更多信息，请参阅 Java 平台标准版部署指南中的[Codebase Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG897)。

+   `Application-Name`属性用于提供签名应用程序安全提示中显示的标题。有关更多信息，请参阅 Java 平台标准版部署指南中的[Application-Name Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG899)。

+   `Application-Library-Allowable-Codebase`属性用于标识您的应用程序预期被找到的位置。使用此属性可减少 JAR 文件位于不同位置时在安全提示中显示的位置数量。有关更多信息，请参阅 Java 平台标准版部署指南中的[Application-Library-Allowable-Codebase Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG900)。

+   `Caller-Allowable-Codebase`属性用于标识可以从中 JavaScript 代码调用您的应用程序的域。使用此属性可防止未知的 JavaScript 代码访问您的应用程序。有关更多信息，请参阅 Java 平台标准版部署指南中的[Caller-Allowable-Codebase Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG901)。

+   `Entry-Point`属性用于标识允许用作 RIA 入口点的类。使用此属性可防止未经授权的代码从 JAR 文件中的其他可用入口点运行。有关更多信息，请参阅 Java 平台标准版部署指南中的[Entry-Point Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG902)。

+   `Trusted-Only`属性用于防止加载不受信任的组件。有关更多信息，请参阅 Java 平台标准版部署指南中的[Trusted-Only Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG903)。

+   `Trusted-Library`属性用于允许特权 Java 代码和沙箱 Java 代码之间的调用，而无需提示用户进行权限确认。有关更多信息，请参阅 Java 平台标准版部署指南中的[Trusted-Library Attribute](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html#JSDPG904)。

有关如何将这些属性添加到清单文件中的信息，请参阅修改清单文件。
