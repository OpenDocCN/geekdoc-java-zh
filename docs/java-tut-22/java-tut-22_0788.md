# 外部资源

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/properties/resources.html`](https://docs.oracle.com/javase/tutorial/jaxp/properties/resources.html)

XML、Schema 和 XSLT 标准支持以下需要外部资源的构造。JDK XML 处理器的默认行为是建立连接并按照指定的方式获取外部资源。

+   外部 DTD：引用外部文档类型定义（DTD），示例：`<!DOCTYPE root_element SYSTEM "url">`

+   外部实体引用：引用外部数据，语法：`<!ENTITY name SYSTEM "url">`

    通用实体引用如下：

    ```java

    <?xml version="1.0" standalone="no" ?>
    <!DOCTYPE doc [<!ENTITY otherFile SYSTEM "otherFile.xml">]>
    <doc>
        <foo>
        <bar>&otherFile;</bar>
        </foo>
    </doc>

    ```

+   外部参数实体，语法 `<!ENTITY % name SYSTEM uri>`。例如：

    ```java
    <?xml version="1.0" standalone="no"?>
        <!DOCTYPE doc [
          <!ENTITY % foo SYSTEM "http://www.example.com/student.dtd"<
          %foo;
        ]>

    ```

+   XInclude：在 XML 文档中包含外部信息集

+   使用 `schemaLocation` 属性、`import` 和 `include` 元素引用 XML Schema 组件。示例：`schemaLocation="http://www.example.com/schema/bar.xsd"`

+   使用 `import` 或 `include` 元素合并样式表：语法：`<xsl:include href="include.xsl"/>`

+   xml-stylesheet 处理指令：用于在 xml 文档中包含样式表，语法：`<?xml-stylesheet href="foo.xsl" type="text/xsl"?>`

+   XSLT `document()` 函数：用于访问外部 XML 文档中的节点。例如，`<xsl:variable name="dummy" select="document('DocumentFunc2.xml')/>`。
