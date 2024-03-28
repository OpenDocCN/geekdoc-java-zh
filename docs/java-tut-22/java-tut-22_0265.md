# 使用 Applet 标签部署

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/html.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/html.html)

如果您不确定最终用户的浏览器是否启用了 JavaScript 解释器，您可以通过手动编码 `<applet>` HTML 标签来部署您的 Java applet，而不是使用 Deployment Toolkit 函数。根据您需要支持的浏览器，您可能需要使用 `<object>` 或 `<embed>` HTML 标签来部署您的 Java applet。查看[W3C HTML 规范](http://www.w3.org/TR/1999/REC-html401-19991224/)以获取有关这些标签的使用详情。

您可以使用 Java 网络启动协议（JNLP）启动您的 applet，或者直接在 `<applet>` 标签中指定启动属性。

## 部署准备

按照部署 applet 主题中描述的步骤编译您的源代码，创建并签名 JAR 文件，如有必要创建 JNLP 文件。部署的整体步骤仍然相关。只有包含 applet 的 HTML 页面的内容会发生变化。

## 手动编码 Applet 标签，使用 JNLP 启动

`AppletPage_WithAppletTag.html` 页面使用手动编码的 `<applet>` 标签部署了 Dynamic Tree Demo applet（意味着该 applet 不是使用自动生成所需 HTML 的 Deployment Toolkit 部署的）。该 applet 仍然使用 JNLP 启动。JNLP 文件在 `jnlp_href` 属性中指定。

```java
<applet code = 'appletComponentArch.DynamicTreeApplet' 
        jnlp_href = 'dynamictree_applet.jnlp'
        width = 300
        height = 300 />

```

## 手动编码 Applet 标签，无需使用 JNLP 启动

使用 JNLP 是部署 applet 的首选方式，但您也可以在没有 JNLP 文件的情况下部署您的 applet。

`AppletPage_WithAppletTagNoJNLP.html` 部署了 Dynamic Tree Demo applet，如下面的代码片段所示。

```java
<applet code = 'appletComponentArch.DynamicTreeApplet' 
    archive = 'DynamicTreeDemo.jar'
    width = 300
    height = 300>
    <param name="permissions" value="sandbox" />
</applet>

```

其中

+   `code` 是 applet 类的名称。

+   `archive` 是包含 applet 及其资源的 jar 文件的名称。

+   `width` 是 applet 的宽度。

+   `height` 是 applet 的高度。

+   `permissions` 表示 applet 是否在安全沙箱中运行。将值指定为"sandbox"以在沙箱中运行。将值指定为"all-permissions"以在沙箱外运行。如果未提供 `permissions` 参数，则签名的 applet 默认为"all-permissions"，未签名的 applet 默认为"sandbox"。
