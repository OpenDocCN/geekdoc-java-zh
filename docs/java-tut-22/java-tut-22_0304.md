# 部署工具包

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/depltoolkit_index.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/depltoolkit_index.html)

部署工具包脚本是一组 JavaScript 函数，可以帮助开发人员在各种浏览器和操作系统配置中一致地部署丰富的互联网应用程序（RIA）。部署工具包脚本评估底层浏览器和操作系统，并使用正确的 HTML 部署 RIA。此脚本还可以确保客户端计算机上存在所需版本的 Java 运行时环境（JRE）软件。部署工具包脚本是在 Java 平台标准版 6 更新 10 发布中引入的。

## 部署工具包脚本位置

部署工具包脚本位于以下网址：

+   `**http:**//www.java.com/js/deployJava.js`（见 note）。

+   `**https:**//www.java.com/js/deployJava.js` – 在安全页面部署您的小程序时，请使用来自此安全位置的部署工具包脚本，以避免页面加载时出现混合内容警告。

* * *

**注意：** `**http:**//www.java.com/js/deployJava.js` 网址正在逐步淘汰。请使用 `**https:**//www.java.com/js/deployJava.js` 网址启动所有应用程序。

* * *

此位置的 JavaScript 代码已经被最小化，以便快速加载。您可以在[`https://www.java.com/js/deployJava.txt`](https://www.java.com/js/deployJava.txt)查看带有相关注释块的 JavaScript 代码的可读版本。

* * *

**注意：** 客户端浏览器应启用 JavaScript 解释器，以便部署工具包脚本能够正确运行并部署您的 RIA。

* * *
