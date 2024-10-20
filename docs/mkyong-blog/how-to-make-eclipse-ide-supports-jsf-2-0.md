# 如何让 Eclipse IDE 支持 JSF 2.0

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/how-to-make-eclipse-ide-supports-jsf-2-0/>

在 Eclipse Ganymede(3.4 版)或 Galileo(3.5 版)中，它只支持 JSF 1.2 版。对于 **JSF 2.0** ，把你的 Eclipse 升级到**Helios(3.6)**版本以后，它已经完全支持 Java EE 6 支持，包括 JSF 2.0。

这里有一个快速指南，向您展示如何在 Eclipse IDE 中启用 JSF 2.0 的特性，比如代码辅助和可视化 JSF 组件编辑器。

使用的工具

1.  Eclipse 3.6
2.  JSF 2.0.x

## 1.Eclipse 项目方面

为了支持 JSF 2.0，你需要配置 Eclipse 项目来支持 **Web 工具平台(WTP)** 。

启用网络工具平台的步骤(WTP):

1.  右击项目，选择“**属性**”—>**“项目刻面**”。
2.  勾选**动态网页模块**，选择版本 2.5。
3.  勾选 **Java** ，选择版本 1.6。
4.  勾选“ **JavaServer Faces** ，选择版本 2.0。【T2![eclipse-jsf-support](img/a53985277428079c5a628d218a13fa0b.png "eclipse-jsf-support-1")
5.  点击下面的“**进一步配置…** ”链接进行 JSF 配置。
6.  创建一个用户库，包括 JSF 2.0 API 和实现库， **jsf-api-xxx.jar** 和 **jsf-impl-xxx.jar** 。
    *P.S 你可以在 JSF 官方网站获得 JSF 坛子[。](http://web.archive.org/web/20220116154214/http://javaserverfaces.java.net/)*![eclipse-jsf-support](img/2f4bf19f66670341f86f337704130d42.png "eclipse-jsf-support-2")**2012 年 8 月 8 日更新**
    对于 JSF 2.1.11，只需要一个 jar 文件 javax.faces-2.1.11。
7.  完成了。

## 2.演示

现在，Eclipse IDE 支持 JSF 2.0 的功能。试试看，在`.xhtml`文件中，点击“ **Ctrl +空格键**，会自动提示所有可用的 JSF 2.0 标签(代码辅助)。

此外，它还在网页编辑器中添加了 JSF 2.0 可视化组件，见下图:

![eclipse-jsf-support](img/06f2fbf1ee4a65bb1c19c5cd1b7ad2d9.png "eclipse-jsf-support-3")<input type="hidden" id="mkyong-current-postId" value="6996">