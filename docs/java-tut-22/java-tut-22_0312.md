# JNLP 文件的结构

> 原文：[`docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/jnlpFileSyntax.html`](https://docs.oracle.com/javase/tutorial/deployment/deploymentInDepth/jnlpFileSyntax.html)

本主题描述了用于丰富互联网应用程序（RIA）的 Java 网络启动协议（JNLP）文件的语法。

下面的代码片段显示了一个 Java Web Start 应用程序的示例 JNLP 文件：

```java
<?xml version="1.0" encoding="UTF-8"?>
<jnlp spec="1.0+" codebase="" href="">
    <information>
        <title>Dynamic Tree Demo</title>
        <vendor>Dynamic Team</vendor>
        <icon href="sometree-icon.jpg"/>
        <offline-allowed/>
    </information>
    <resources>
        <!-- Application Resources -->
        <j2se version="1.6+" href=
           "http://java.sun.com/products/autodl/j2se"/>
        <jar href="DynamicTreeDemo.jar"
            main="true" />

    </resources>
    <application-desc
         name="Dynamic Tree Demo Application"
         main-class="webstartComponentArch.DynamicTreeApplication"
         width="300"
         height="300">
     </application-desc>
     <update check="background"/>
</jnlp>

```

下表描述了 JNLP 文件中常用的元素和属性。单击父链接查看元素的父级。

* * *

**注意：** 此表格不包括 JNLP 文件的所有可能内容。有关更多信息，请参阅[Java 网络启动协议和 API 规范更改日志](http://www.oracle.com/technetwork/java/javase/jnlp-spec-log-139509.html)。

* * *

**JNLP 文件中常用的元素和属性**

| 元素 | 属性 | 描述 | 自版本 | 必需 |
| --- | --- | --- | --- | --- |
| jnlp |  | JNLP 文件的最顶层 xml 元素。 | 1.0  | 是 |
|  | spec | 属性的值可以是 1.0、1.5 或 6.0，也可以使用通配符，如 1.0+。表示此 JNLP 文件可以使用的 JNLP 规范的最低版本。 | 1.0 |  |
|  | codebase | JNLP 文件中 `href` 属性中指定的所有相对 URL 的基本位置。 | 1.0 |  |
|  | href | JNLP 文件本身的 URL。 | 1.0 |  |
|  | version | 正在启动的 RIA 的版本，以及 JNLP 文件本身的版本。 | 1.0 |  |
|      information ^(parent) |  | 包含描述 RIA 及其来源的其他元素。 | 1.0 | 是 |

|  | os | 应考虑的操作系统。 | 1.5.0 |  |

|  |
| --- |
|  | arch | 应考虑的体系结构。 | 1.5.0 |  |
|  | platform | 应考虑的平台。 | 1.5.0 |  |
|  | locale | 应考虑的区域设置。 | 1.5.0 |  |
|          title ^(parent) |  | RIA 的标题。 | 1.0 | 是 |
|          vendor ^(parent) |  | RIA 的提供者。 | 1.0 | 是 |
|          homepage ^(parent) |  | RIA 的主页。 | 1.0 |  |
|  | href | 指向有关此 RIA 的更多信息的 URL。 | 1.0 | 是 |
|          description ^(parent) |  | 描述 RIA 的简短语句。 | 1.0 |  |
|  | kind | 描述类型的指示符。合法值为 one-line、short 和 tooltip。 | 1.0 |  |
|          icon ^(parent) |  | 用于向用户识别 RIA 的图标。 | 1.0 |  |
|  | href | 指向图标文件的 URL。可以是以下格式之一：gif、jpg、png、ico。 | 1.0 | 是 |
|  | kind | 指示图标的建议用途，可以是：default、selected、disabled、rollover、splash 或 shortcut。 | 1.0 |  |
|  | 宽度 | 可用于指示图像的分辨率。 | 1.0 |  |
|  | 高度 | 可用于指示图像的分辨率。 | 1.0 |  |
|  | 深度 | 可用于指示图像的分辨率。 | 1.0 |  |
|          offline-allowed ^(父级) |  | 表示此 RIA 可在客户端系统与网络断开连接时运行。 | 1.0 |  |
|          快捷方式 ^(父级) |  | 可用于指示 RIA 对桌面集成的偏好。 | 1.5.0 |  |
|  | 在线 | 可用于描述 RIA 对创建在线或离线运行快捷方式的偏好。 | 1.5.0 |  |
|          桌面 ^(父级) |  | 可用于指示 RIA 将快捷方式放置在用户桌面上的偏好。 | 1.5.0 |  |
|          菜单 ^(父级) |  | 可用于指示 RIA 将菜单项放置在用户开始菜单中的偏好。 | 1.5.0 |  |
|  | 子菜单 | 可用于指示 RIA 对菜单项放置位置的偏好。 | 1.5.0 |  |
|          关联 ^(父级) |  | 可用于提示 JNLP 客户端，RIA 希望在操作系统中注册为某些扩展和某些 MIME 类型的主要处理程序。如果包括此元素，则必须同时包括 offline-allowed 元素，或者必须为 jnlp 元素设置 href 属性。 | 1.5.0 |  |
|  | 扩展 | RIA 请求注册处理的文件扩展名列表（以空格分隔）。 | 1.5.0 |  |
|  | MIME 类型 | RIA 请求注册处理的 MIME 类型。 | 1.5.0 |  |
|          相关内容 ^(父级) |  | 可与 RIA 集成的附加相关内容。 | 1.5.0 |  |
|  | 链接 | 指向相关内容的 URL。 | 1.5.0 | 是 |
|     更新 ^(父级) |  | JNLP 客户端应如何处理 RIA 更新的偏好。 | 1.6.0 |  |
|  | 检查 | JNLP 客户端应何时检查更新的偏好。值可以是 always、timeout 或 background。 | 1.6.0 |  |
|  | 策略 | JNLP 客户端在启动 RIA 之前处理新版本可用时的 RIA 更新偏好。值可以是 always、prompt-update 或 prompt-run。 | 1.6.0 |  |
|  |  |  | 1.0 |  |
|     安全 ^(父级) |  | 可用于请求增强权限。如果不包括此元素，则应用程序在安全沙箱中运行。 | 1.0 |  |
|          all-permissions ^(父级) |  | 请求以所有权限运行 RIA。 | 1.0 |  |
|          j2ee-application-client-permissions ^(父级) |  | 请求以符合 J2EE 应用程序客户端环境安全规范的权限集运行 RIA。 | 1.0 |  |
|     资源 ^(父级) |  | 描述 RIA 需要的所有资源。 | 1.0 | 是 |
|  | os | 应考虑资源元素的操作系统。 | 1.0 |  |
|  | arch | 应考虑资源元素的架构。 | 1.0 |  |
|  | locale | 应考虑资源元素的区域设置。 |  |  |
|          java or j2se ^(parent) |  | 用于运行 RIA 的 Java 软件版本。 | 1.6.0 (java)  |  |
|  | version | 要使用的版本范围的有序列表。 | 1.0 | 是 |
|  | href | 指示此版本的 Java 软件供应商的 URL，以及可以从哪里下载。 | 1.0 |  |
|  | java-vm-args | RIA 希望 JNLP 客户端在启动 JRE 软件时使用的一组标准和非标准虚拟机参数。 | 1.0 |  |
|  | initial-heap-size | Java 堆的初始大小。 | 1.0 |  |
|  | max-heap-size | Java 堆的最大大小。 | 1.0 |  |
|          jar ^(parent) |  | RIA 类路径的 JAR 文件。 | 1.0 | 是 |
|  | href | JAR 文件的 URL。 | 1.0 | 是 |
|  | version | JAR 文件的请求版本。需要使用基于版本的下载协议 | 1.0 |  |
|  | main | 表示此 JAR 文件是否包含 RIA 的包含`main`方法的类。 | 1.0 |  |
|  | download | 表示此 JAR 文件可以懒加载下载，或在需要时下载。 | 1.0 |  |
|  | size | JAR 文件的可下载大小（以字节为单位）。 | 1.0 |  |
|  | part | 可用于将资源分组在一起，以便同时下载。 | 1.0 |  |
|          nativelib ^(parent) |  | 包含本机库的 JAR 文件，位于其根目录中。 | 1.0 |  |
|  | href | JAR 文件的 URL。 | 1.0 | 是 |
|  | version | JAR 文件的请求版本。需要使用基于版本的下载协议 | 1.0 |  |
|  | download | 可用于指示此 JAR 文件可以懒加载下载。 | 1.0 |  |
|  | size | JAR 文件的可下载大小（以字节为单位）。 | 1.0 |  |
|  | part | 可用于将资源分组在一起，以便同时下载。 | 1.0 |  |
|          extension ^(parent) |  | 指向要与此 RIA 一起使用的附加 component-desc 或 installer-desc。 | 1.0 |  |
|  | href | 附加扩展 JNLP 文件的 URL。 | 1.0 | 是 |
|  | version | 附加扩展 JNLP 文件的版本。 | 1.0 |  |
|  | name | 附加扩展 JNLP 文件的名称 | 1.0 |  |
|              ext-download ^(parent) |  | 可用于在扩展元素中表示包含在组件扩展中的部分。 | 1.0 |  |
|  | ext-part | 可预期在扩展中找到的部分的名称。 | 1.0 | 是 |
|  | download | 可用于指示此扩展可以急切或懒加载下载。 | 1.0 |  |
|  | part | 指示在此 JNLP 文件中包含扩展的部分的名称。 | 1.0 |  |
|          package ^(parent) |  | 可用于指示 JNLP 客户端哪些包实现在哪些 JAR 文件中。 | 1.0 |  |
|  | name | 给定部分的 JAR 文件中包含的包名。 | 1.0 | 是 |
|  | part | 包含包名的 JAR 文件的部分名称。 | 1.0 | 是 |
|  | recursive | 可用于指示所有以给定名称开头的包名可以在给定部分中找到。 | 1.0 |  |
|          property ^(parent) |  | 定义一个系统属性，可以通过`System.getProperty`和`System.getProperties`方法获得。 | 1.0 |  |
|  | name | 系统属性的名称。 | 1.0 | 是 |
|  | value | 系统属性的值。 | 1.0 | 是 |
|  |  | 注意：JNLP 文件必须包含以下之一：application-desc、applet-desc、component-desc 或 installer-desc。 | 1.0 | 是 |
|     application-desc ^(parent) |  | 表示这是一个应用程序的 JNLP 文件。 | 1.0 |  |
|  | main-class | 应用程序的包含`public static void main(String[])`方法的类的名称。 | 1.0 | 是 |
|          argument ^(parent) |  | 每个参数包含（按顺序）要传递给`main`方法的额外参数。 | 1.0 |  |
|     applet-desc ^(parent) |  | 表示这是一个 applet 的 JNLP 文件。 | 1.0 |  |
|  | main-class | 主 applet 类的名称。 | 1.0 | 是 |
|  | documentbase | 应用程序的 applet 的文档基础 URL。 | 1.0 |  |
|  | name | applet 的名称。 | 1.0 | 是 |
|  | width | applet 的宽度（以像素为单位）。 | 1.0 | 是 |
|  | height | applet 的高度（以像素为单位）。 | 1.0 | 是 |
|          param ^(parent) |  | 可传递给 applet 的一组参数。 | 1.0 |  |
|  | name | 此参数的名称。 | 1.0 | 是 |
|  | value | 此参数的值。 | 1.0 | 是 |
|     component-desc ^(parent) |  | 表示这是一个组件扩展的 JNLP 文件。 | 1.0 |  |
|     installer-desc ^(parent) |  | 表示这是一个已安装扩展的 JNLP 文件。 | 1.0 |  |
|  | main-class | 安装程序的包含`public static void main(String[])`方法的类的名称。 | 1.0 | 是 |

## 编码 JNLP 文件

Java Web Start 软件支持在 Java 平台支持的任何字符编码中对 JNLP 文件进行编码。有关 Java 平台中字符编码的更多信息，请参阅[支持的编码指南](https://docs.oracle.com/javase/8/docs/technotes/guides/intl/encoding.doc.html)。要对 JNLP 文件进行编码，请在该文件的 XML prolog 中指定一个编码。例如，以下行表示 JNLP 文件以 UTF-16 编码。

```java
<?xml version="1.0" encoding="utf-16"?>

```

* * *

**注意：** XML prolog 本身必须是 UTF-8 编码的。

* * *
