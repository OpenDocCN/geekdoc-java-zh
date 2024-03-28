# 为什么使用 JMX 技术？

> 原文：[`docs.oracle.com/javase/tutorial/jmx/overview/why.html`](https://docs.oracle.com/javase/tutorial/jmx/overview/why.html)

JMX 技术为开发人员提供了一种灵活的手段来为基于 Java 技术的应用程序（Java 应用程序）提供仪器化，创建智能代理，实现分布式管理中间件和管理器，并将这些解决方案顺利集成到现有的管理和监控系统中。

+   **JMX 技术使得 Java 应用程序可以在不需要大量投资的情况下进行管理**。

    基于 JMX 技术的代理（JMX 代理）可以在大多数支持 Java 技术的设备上运行。因此，Java 应用程序可以在设计上几乎没有影响地变得可管理。一个 Java 应用程序只需要嵌入一个托管对象服务器，并将部分功能作为一个或多个托管 bean（MBeans）注册到对象服务器中。这就是从管理基础设施中受益所需的全部。

+   **JMX 技术提供了一种标准的方式来管理 Java 应用程序、系统和网络**。

    例如，Java 平台企业版（Java EE）5 应用服务器符合 JMX 架构，因此可以使用 JMX 技术进行管理。

+   **JMX 技术可用于对 Java 虚拟机进行开箱即用的管理**。

    Java 虚拟机（Java VM）使用 JMX 技术进行高度仪器化。您可以启动一个 JMX 代理来访问内置的 Java VM 仪器，从而远程监视和管理 Java VM。

+   **JMX 技术提供了一个可扩展的、动态的管理架构**。

    每个 JMX 代理服务都是一个独立的模块，可以根据需求插入到管理代理中。这种基于组件的方法意味着 JMX 解决方案可以从小型设备扩展到大型电信交换机等更大的设备。JMX 规范提供了一组核心代理服务。可以开发额外的服务，并在管理基础设施中动态加载、卸载或更新这些服务。

+   **JMX 技术利用了现有的标准 Java 技术**。

    在需要时，JMX 规范引用现有的 Java 规范，例如 Java 命名和目录接口（J.N.D.I.）API。

+   **基于 JMX 技术的应用程序（JMX 应用程序）可以通过 NetBeans IDE 模块创建**。

    您可以从 NetBeans 更新中心获取一个模块（在 NetBeans 界面中选择工具 -> 更新中心），该模块使您可以使用 NetBeans IDE 创建 JMX 应用程序。这降低了 JMX 应用程序开发的成本。

+   **JMX 技术与现有的管理解决方案和新兴技术集成**。

    JMX API 是任何管理系统供应商都可以实现的开放接口。JMX 解决方案可以使用查找和发现服务以及诸如 Jini 网络技术和服务位置协议（SLP）等协议。