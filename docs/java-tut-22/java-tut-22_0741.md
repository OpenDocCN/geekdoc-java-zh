# 教训：JMX 技术概述

> 原文：[`docs.oracle.com/javase/tutorial/jmx/overview/index.html`](https://docs.oracle.com/javase/tutorial/jmx/overview/index.html)

Java 管理扩展（JMX）技术是 Java 平台标准版（Java SE 平台）的标准部分。JMX 技术是在 Java 2 平台标准版（J2SE）5.0 发布中添加到平台中的。

JMX 技术提供了一种简单、标准的管理资源（如应用程序、设备和服务）的方式。由于 JMX 技术是动态的，您可以使用它来监视和管理资源的创建、安装和实施过程。您还可以使用 JMX 技术来监视和管理 Java 虚拟机（Java VM）。

JMX 规范为管理和监视应用程序和网络定义了 Java 编程语言中的架构、设计模式、API 和服务。

使用 JMX 技术，一个给定的资源由一个或多个称为*托管 Bean*或*MBeans*的 Java 对象仪器化。这些 MBeans 注册在一个核心管理对象服务器中，称为*MBean 服务器*。MBean 服务器充当管理代理，并可以在大多数已启用 Java 编程语言的设备上运行。

规范定义了您用于管理已正确配置的任何资源的 JMX 代理。JMX 代理由一个 MBean 服务器（其中注册了 MBeans）和一组用于处理 MBeans 的服务组成。通过这种方式，JMX 代理直接控制资源并使其可供远程管理应用程序使用。

资源的仪器化方式与管理基础设施完全独立。因此，资源可以被管理，而不管它们的管理应用是如何实现的。

JMX 技术定义了标准连接器（称为 JMX 连接器），使您可以从远程管理应用程序访问 JMX 代理。使用不同协议的 JMX 连接器提供相同的管理接口。因此，管理应用程序可以透明地管理资源，而不管使用的通信协议是什么。只要这些系统或应用程序支持 JMX 代理，JMX 代理也可以被不符合 JMX 规范的系统或应用程序使用。
