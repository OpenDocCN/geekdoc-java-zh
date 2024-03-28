# 用户接受 RIA

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/userAcceptance.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/userAcceptance.html)

为了安全起见，在首次启动 RIA 之前，用户会被提示是否允许运行 RIA，即使应用程序已签名或不需要超出安全沙箱的访问权限。提示包括以下信息，取决于正在运行的 RIA：

+   RIA 的名称，或者通知应用程序未签名。

+   如果应用程序使用来自受信任机构颁发的证书进行签名，则会显示有关发布者的信息。如果证书已过期，则会包含警告。如果应用程序是自签名的，则发布者显示为 UNKNOWN。

+   如果证书已过期、被吊销或无法检查吊销状态，则会收到警告。

+   应用程序访问的位置。

+   应用程序所需的访问级别。有限访问将应用程序限制在安全沙箱中，无限制访问将使应用程序能够访问用户系统上的资源。

+   如果建议的属性不存在，则会警告缺少 JAR 文件清单属性。

+   对于未签名或自签名的应用程序，用户必须选择接受应用程序的复选框。

+   在某些情况下，有选择不再显示提示的选项。

有关提示的描述，请参阅[当我看到 Java 的安全提示时应该怎么做？](http://java.com/faq-securityprompts)

如果用户运行的 JRE 版本过时，他们也会收到警告，并有机会在运行应用程序之前更新到最新版本。用户还可以选择使用系统上的 JRE 运行，或者阻止应用程序运行。

Java 控制面板中的安全级别设置确定用户是否有机会运行 RIA。默认设置为高，会提示用户是否允许运行使用有效证书签名并在主 JAR 文件的清单中包含 Permissions 属性的应用程序。如果无法检查应用程序的吊销状态，则用户还可以在用户的许可下运行应用程序。

对 RIA 进行签名为用户提供了一定程度的信任。在准备应用程序部署时，请考虑以下事项：

+   最佳用户体验是由使用受认可证书颁发机构颁发的证书签名的应用程序提供的。

+   除非已创建例外站点列表或部署规则集以明确允许应用程序运行，否则不允许运行自签名和未签名的应用程序。

+   签名应用程序可以是特权应用程序或沙箱应用程序。特权应用程序可以无限制地访问用户系统上的资源。沙箱应用程序受限于 Java 安全沙箱。未签名应用程序受限于沙箱。