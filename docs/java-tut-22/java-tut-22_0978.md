# LDAP 设置

> 原文：[`docs.oracle.com/javase/tutorial/jndi/software/content.html`](https://docs.oracle.com/javase/tutorial/jndi/software/content.html)

下面是构建访问 LDAP 目录服务器的 Java 应用程序涉及的步骤。

1.  安装 Java 平台软件。

1.  获取目录服务器软件，如之前讨论过的。

1.  配置目录服务器以使用所需的模式。在本教程中使用的示例需要在服务器上配置一个特殊的 schema。

1.  使用所需的内容填充目录服务器。在本教程中使用的示例需要在服务器上填充一个特殊的内容。

1.  编写一个 JNDI 应用程序来访问目录，编译并运行它以获得您想要的结果。JNDI 示例将在下一个课程中介绍。

*前两个*步骤在前一部分中已经涵盖。本课程的其余部分讨论了*第三步*和*第四步*的一部分。涉及编写 JNDI 应用程序的*第五步*在下一课程中介绍，展示如何编写 JNDI 应用程序来执行目录上的各种操作。

一旦您设置了目录，或者已经指示您的程序与现有目录通信，您可以在那里找到什么样的信息？

目录可以被视为由名称到对象绑定组成。也就是说，目录中的每个对象都有一个对应的名称。您可以通过查找其名称来检索目录中的对象。

目录中还存储着属性。目录中的对象除了有一个名称外，还有一组可选的属性。您可以向目录查询对象的属性，也可以要求它搜索具有特定属性的对象。

## 步骤 3：目录模式

模式指定目录可能包含的对象类型。本教程使用条目填充目录，其中一些条目需要特殊的模式定义。为了容纳这些条目，您必须首先在服务器中关闭模式检查，或者将附带本教程的模式文件添加到服务器中。这两项任务通常由目录服务器的管理员执行。

本教程附带两个必须安装的模式文件：

+   `Java 对象的模式`

+   `CORBA 对象的模式`

这些文件的格式是一种可能不能直接复制粘贴到服务器配置文件中的正式描述。具体来说，属性语法是根据[RFC 2252](http://www.ietf.org/rfc/rfc2252.txt)描述的。

不同的目录服务器有不同的配置模式方式。本教程包括一些工具，用于在允许通过 LDAP 修改其模式的目录服务器上安装 Java 和 CORBA 模式。以下是工具可以执行的任务列表。

1.  `创建 Java 模式`

1.  `创建 CORBA 模式`

按照随附的`README 文件`中的说明运行这些程序。

* * *

**注意：Windows 活动目录。** Active Directory 通过使用内部格式管理其模式。要更新模式，您可以使用 Active Directory 管理控制台插件`ADSIEdit`或`CreateJavaSchema`实用程序，按照 Active Directory 的说明进行操作。

* * *

## 第 4 步：为本教程提供目录内容

在本教程的示例中，显示的结果反映了使用随附本教程的配置文件（`tutorial.ldif`）设置 LDAP 目录的方式。如果您正在使用现有服务器或具有不同设置的服务器，则可能会看到不同的结果。在将配置文件（`tutorial.ldif`）加载到目录服务器之前，您必须按照更新服务器模式的说明进行操作，或者如果您的 UNIX 系统上有*ldapadd*或*ldapmodify*命令，则可以使用它们。

例如，使用 ldapmodify，您可以执行以下操作（通过为主机名、管理员 DN（-D 选项）和密码插入适当的值）：

```java
ldapmodify -a -c -v -h hostname -p 389\
        -D "cn=Administrator, cn=users, dc=xxx, dc=xxx"\
        -w passwd -f tutorial.ldif

```

* * *

**安装注意事项：访问控制。** 不同的目录服务器以不同方式处理访问控制。本教程中的一些示例执行对目录的更新。此外，您安装教程的命名空间部分可能具有读取访问限制。因此，您需要采取特定于服务器的操作，使目录可读和/或可更新，以使这些示例正常工作。对于[Oracle 目录服务器](http://www.oracle.com/technetwork/testcontent/index-085178.html)，请将`sunds.aci.ldif`文件中建议的`aci`条目添加到`dn: o=JNDITutorial`条目中，以使整个目录可读和可更新。或者，您可以更改示例以对目录进行身份验证。如何执行此操作的详细信息在安全课程中有描述。

**安装注意事项：命名空间设置。** `tutorial.ldif` 文件中的条目使用了"o=JNDITutorial"作为根命名上下文的区分名称（DN）。如果您尚未将目录服务器配置为具有"o=JNDITutorial"作为根命名上下文，则导入`tutorial.ldif`的尝试将失败。解决此问题的最简单方法是将现有根命名上下文的 DN 添加到`tutorial.ldif`文件中的每个"dn:"行中。例如，如果您的服务器已经具有根命名上下文"dc=imc,dc=org"，则应更改该行

```java
dn: o=JNDITutorial

```

到

```java
dn: o=JNDITutorial, dc=imc, dc=org

```

对文件中以"dn:"开头的每一行进行更改。然后，在本教程的所有示例中，无论何时使用"o=JNDITutorial"，请改用"o=JNDITutorial,dc=imc,dc=org"。

**安装说明：文件格式。**根据您使用的操作系统平台，您可能需要编辑`tutorial.ldif`，以便其中包含该平台的正确换行符。例如，如果您发现`tutorial.ldif`包含 Windows 风格的换行符（CRLF），而您要将此文件导入运行在 UNIX 平台上的目录服务器，则需要编辑文件并将 CRLF 替换为 LF。此问题的症状是目录服务器拒绝`tutorial.ldif`中的所有条目。

**安装说明：Windows Active Directory。**

1.  根命名上下文不会是“o=jnditutorial”。它将采用“dc=x,dc=y,dc=z”的形式。您需要遵循之前的**命名空间设置**说明。

1.  通过使用 Active Directory 管理控制台插件`ADSIEdit`，为“inetOrgPerson”和“groupOfUniqueNames”对象类添加对象类和相关属性到 Active Directory 模式中。"groupOfUniqueNames"在[RFC 2256](http://www.ietf.org/rfc/rfc2256.txt)中定义，"inetOrgPerson"在[RFC 2798](http://www.ietf.org/rfc/rfc2798.txt)中定义。

1.  教程中使用的一些层次关系在 Active Directory 中默认情况下是不允许的。要启用这些关系，请使用 Active Directory 管理控制台插件`ADSIEdit`添加它们。

    ```java
    objectclass: organizationalUnit
    possible superiors: domainDNS
                        inetOrgPerson
                        organization
                        organizationalPerson
                        organizationalUnit
                        person
                        top

    objectclass: groupOfUniqueNames
    possible superiors: top

    objectclass: inetOrgPerson
    possible superiors: container
                        organizationalPerson
                        person
                        top

    ```

1.  从 `tutorial.ldif` 中的马克·吐温条目中删除两个“sn”属性中的一个。Active Directory 将“sn”定义为单值属性，与[RFC 2256](http://www.ietf.org/rfc/rfc2256.txt)相悖。

1.  使用`ldifde`命令行实用程序加载修改后的`tutorial.ldif`文件。

    ```java
    # ldifde -i -v -k -f tutorial.ldif

    ```

1.  大多数示例假定目录已设置为允许未经身份验证的读取和更新访问。您的 Active Directory 设置可能不允许您这样做。请参阅**访问控制**安装说明。

1.  有时读取条目会产生比教程中显示的更多属性，因为 Active Directory 通常会返回一些内部属性。

1.  创建条目可能需要指定其他 Active Directory 特定属性或使用其他对象类。
