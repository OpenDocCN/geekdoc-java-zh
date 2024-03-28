# 课程：软件设置

> 原文：[`docs.oracle.com/javase/tutorial/jndi/software/index.html`](https://docs.oracle.com/javase/tutorial/jndi/software/index.html)

## 所需软件

以下是您需要的软件/系统列表：

+   Java 平台软件

+   服务提供者软件

+   命名和目录服务器软件

* * *

### Java 平台软件

JNDI 包含在 Java SE 平台中。

要运行小程序，请在 Microsoft Edge 上使用 IE 模式。请参阅[Microsoft Edge + Internet Explorer 模式：入门指南](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWEHMs)。

### 服务提供者软件

JNDI API 是用于访问任何命名或目录服务的通用 API。通过在 JNDI 下插入服务提供者，可以实现对命名或目录服务的实际访问。有关 JNDI 架构和服务提供者角色的概述，请参阅 JNDI 概述课程。

*服务提供者*是将 JNDI API 映射到对命名或目录服务器的实际调用的软件。通常，服务提供者的角色与命名/目录服务器的角色不同。在客户端/服务器软件术语中，JNDI 和服务提供者是*客户端*（称为*JNDI 客户端*），而命名/目录服务器是*服务器*。

客户端和服务器可以以许多方式进行交互。在一种常见的方式中，它们使用网络协议，以便客户端和服务器可以在网络环境中独立存在。服务器通常支持许多不同的客户端，不仅限于 JNDI 客户端，只要客户端符合指定的协议。JNDI 不规定 JNDI 客户端和服务器之间的任何特定交互方式。例如，在一个极端情况下，客户端和服务器可以是同一实体。

您需要获取将要使用的服务提供者的类。例如，如果您计划使用 JNDI 访问 LDAP 目录服务器，则需要 LDAP 服务提供者的软件。

JDK 附带以下服务提供者：

+   轻量级目录访问协议（LDAP）

+   CORBA 公共对象服务命名（COS 命名）

+   RMI 注册表

+   域名服务（DNS）

如果您对其他提供者感兴趣，请查看[JNDI 页面](http://www.oracle.com/technetwork/java/jndi/index.html)以获取下载信息。

本教程仅使用 LDAP 服务提供者。使用 LDAP 服务提供者时，您需要设置自己的服务器或访问现有服务器，如下所述。

### 命名和目录服务器软件

一旦您获得了服务提供商软件，您就需要设置或访问相应的命名/目录服务器。设置命名/目录服务器通常是网络系统管理员的工作。不同的供应商对其命名/目录服务器有不同的安装程序。有些在服务器安装之前需要特殊的机器权限。您应该查阅命名/目录服务器软件的安装说明。

在本教程中的目录示例中，您需要访问一个 LDAP 服务器。如果您想快速了解 LDAP 是什么，请查看[这里](http://en.wikipedia.org/wiki/LDAP)。您可以使用您选择的任何符合 LDAP 标准的服务器。Oracle Directory Server 在许多平台上运行，包括 Windows，可在以下网址进行评估：[Oracle Directory Server](http://www.oracle.com/technetwork/testcontent/index-085178.html)。

您也可以在下面下载一个免费的 LDAP 服务器：

+   [389 目录服务器](http://directory.fedoraproject.org/)

+   [Apache 目录服务器](http://directory.apache.org)

+   [OpenLDAP](http://www.OpenLDAP.org/)

一个公开访问的服务器位于：ldap://ldap.openldap.org 命名上下文：dc=OpenLDAP,dc=org
