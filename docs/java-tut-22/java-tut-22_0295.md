# JNLP API

> 原文：[`docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/jnlpAPI.html`](https://docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/jnlpAPI.html)

富互联网应用程序（RIAs）可以使用 Java 网络启动协议（JNLP）API 对用户环境执行广泛的操作。当使用 JNLP 启动时，即使未签名的 RIAs 也可以在用户许可的情况下执行以下操作：

+   他们可以使用[`FileOpenService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/FileOpenService.html)和[`FileSaveService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/FileSaveService.html) API 访问用户的文件系统。

+   他们可以使用[`ClipboardService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/ClipboardService.html) API 访问共享的系统剪贴板。

+   他们可以使用[`PrintService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/PrintService.html) API 访问打印功能。

+   他们可以使用[`PersistenceService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/PersistenceService.html) API 访问持久性存储。

+   他们可以使用[`DownloadService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/DownloadService.html) API 控制 RIA 的下载和缓存方式。

+   他们可以使用[`DownloadServiceListener`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/DownloadServiceListener.html) API 确定 RIA 下载的进度。

+   他们可以使用[`SingleInstanceService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/SingleInstanceService.html) API 决定在启动多个 RIA 实例时如何处理参数。

+   他们可以使用[`ExtendedService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/ExtendedService.html) API 请求权限打开以前未打开过的某些文件。

查看[JNLP API 文档](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/package-summary.html)以查看通过 JNLP 启动的 RIA 可用功能的完整列表。
