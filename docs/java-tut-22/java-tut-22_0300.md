# 保护富互联网应用程序的指南

> [`docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/devGuidelines.html`](https://docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/devGuidelines.html)

以下指南提供了您可以采取的步骤，以减少您向用户提供的富互联网应用程序（RIA）的漏洞性。

+   遵循安全编码指南

+   使用最新版本的 JRE 进行测试

+   包含清单属性

+   使用已签名的 JNLP 文件

+   签名和时间戳 JAR 文件

+   使用 HTTPS 协议

+   避免本地 RIA

## 遵循安全编码指南

遵循[Java 编程语言安全编码指南](http://www.oracle.com/technetwork/java/seccodeguide-139067.html)中的建议。第 4 节“可访问性和可扩展性”描述了如何限制对类和包的访问权限，从而降低代码的漏洞性。

JavaScript 代码被认为是不安全的，并且默认情况下受到安全沙箱的限制。最小化您的 RIA 与 JavaScript 代码之间的交互。谨慎使用`AccessController.doPrivileged`块，因为它允许从任何 HTML 页面或 JavaScript 代码访问。

## 使用最新版本的 JRE 进行测试

确保您的 RIA 在最新的安全版本的 JRE 上运行。Java 平台支持 RIA 指定运行 RIA 所需的 Java 版本的能力，然而，要求用户维护多个 JRE 版本，尤其是旧的、不安全的版本，对用户来说是一个安全风险。

RIA 的一个好处是更新版本的 RIA 会自动下载到用户的系统中。测试您的 RIA 与每个 JRE 更新版本，并确保其正常工作。如果需要更改，请在服务器上更新您的 RIA，以便用户可以安装最新的 JRE 并仍然运行 RIA。

## 包含清单属性

向 JAR 文件清单添加属性，描述 RIA 的属性。与 JNLP 文件或`applet`标记中的值进行比较，以验证运行正确的代码。

当您的 RIA 不需要超出安全沙箱的访问权限时，请请求沙箱权限。Java 沙箱为用户提供了额外的保护，如果用户不理解为什么需要无限制访问其系统，则可能不运行特权应用程序。

清单属性还可用于标识可以访问您的 RIA 的位置。这包括 JavaScript 代码可以调用您的 RIA 的位置，以及可以启动您的 RIA 的 JNLP 文件或 applet 标记的位置。有关可用的清单属性的信息，请参阅使用清单属性增强安全性。

## 使用已签名的 JNLP 文件

如果您的 RIA 需要访问非安全系统属性或 JVM 参数，请使用已签名的 JNLP。如果需要外部和内部 JNLP 文件之间的某种变化，请使用 JNLP 模板。查看 [已签名的 JNLP 文件](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/signed_jnlp.html) 了解信息。

要访问非安全系统属性或 JVM 参数，请按照 设置受信任参数和安全属性 中描述的在 JNLP 文件中包含属性或参数。

## 对 JAR 文件进行签名和时间戳

从受信任的证书颁发机构获取代码签名证书，并用它来签署您的 RIA 的 JAR 文件。仅部署使用有效证书签名的 RIA 给用户。

当您对 JAR 文件进行签名时，也要对签名进行时间戳。时间戳验证证书在签署 JAR 时是有效的，因此当证书过期时，RIA 不会自动被阻止。查看 签署 JAR 文件 了解有关签署和时间戳的信息。

自签名和未签名的 RIA 被视为不安全，并且除非设置例外站点列表或部署规则集以允许特定应用程序运行，否则不允许运行。但是，自签名对于测试目的可能是有用的。要使用您的自签名 RIA 进行测试，您可以将自签名证书导入到受信任的密钥库中。

## 使用 HTTPS 协议

使用 HTTPS 协议为用户获取您的 RIA 的 Web 服务器。HTTPS 协议是加密的，并由服务器验证，使任何人更难篡改您的 RIA。

## 避免本地的 RIA

本地的 RIA 不适用于生产环境。为确保用户运行您打算让他们运行的代码，请将您的 RIA 托管在应用程序服务器上。

为了测试，建议使用 Web 服务器。另一个选项是将您的应用程序添加到例外站点列表中，在 Java 控制面板的安全选项卡中进行管理。
