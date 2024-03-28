# 系统属性

> 原文：[`docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/properties.html`](https://docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/properties.html)

本主题列出了受限于安全沙箱并使用或不使用 Java 网络启动协议（JNLP）启动的富互联网应用程序（RIA）可以访问的系统属性。一些系统属性无法被沙箱式富互联网应用程序访问。

## 所有富互联网应用程序都可以检索以下安全系统属性：

所有 RIA 都可以检索以下安全系统属性：

+   ``java.class.version``

+   ``java.vendor``

+   ``java.vendor.url``

+   ``java.version``

+   ``os.name``

+   ``os.arch``

+   ``os.version``

+   ``file.separator``

+   ``path.separator``

+   ``line.separator``

## 通过使用 JNLP 启动的 RIA 可以访问的安全系统属性

通过使用 JNLP 启动的 RIA 可以设置和检索以下安全属性：

+   `awt.useSystemAAFontSettings`

+   `http.agent`

+   `http.keepAlive`

+   `java.awt.syncLWRequests`

+   `java.awt.Window.locationByPlatform`

+   `javaws.cfg.jauthenticator`

+   `javax.swing.defaultlf`

+   `sun.awt.noerasebackground`

+   `sun.awt.erasebackgroundonresize`

+   `sun.java2d.d3d`

+   `sun.java2d.dpiaware`

+   `sun.java2d.noddraw`

+   `sun.java2d.opengl`

+   `swing.boldMetal`

+   `swing.metalTheme`

+   `swing.noxp`

+   `swing.useSystemFontSettings`

## 禁止访问的系统属性

沙箱式富互联网应用程序无法访问以下系统属性：

+   `java.class.path`

+   `java.home`

+   `user.dir`

+   `user.home`

+   `user.name`
