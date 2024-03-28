# 错误处理

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/limits/error.html`](https://docs.oracle.com/javase/tutorial/jaxp/limits/error.html)

建议应用程序在设置新属性时捕获`org.xml.sax.SAXNotRecognizedException`异常，以便应用程序在不支持这些属性的旧版本上正常工作。例如，可下载的示例代码包含以下方法，`isNewPropertySupported`，用于检测示例是否在支持`JDK_GENERAL_ENTITY_SIZE_LIMIT`属性的 JDK 版本上运行：

```java
public boolean isNewPropertySupported() {
    try {
        SAXParser parser = getSAXParser(false, false, false);
        parser.setProperty(JDK_GENERAL_ENTITY_SIZE_LIMIT, "10000");
    } catch (ParserConfigurationException ex) {
        fail(ex.getMessage());
    } catch (SAXException ex) {
        String err = ex.getMessage();
        if (err.indexOf("Property '" + JDK_GENERAL_ENTITY_SIZE_LIMIT +
                                       "' is not recognized.") > -1) {
            //expected before this patch
            debugPrint("New limit properties not supported. Samples not run.");
            return false;
        }
    }
    return true;
}

```

当输入文件包含导致超出限制异常的结构时，应用程序可以检查错误代码以确定失败的性质。以下错误代码适用于这些限制：

+   `EntityExpansionLimit`: JAXP00010001

+   `ElementAttributeLimit`: JAXP00010002

+   `MaxEntitySizeLimit`: JAXP00010003

+   `TotalEntitySizeLimit`: JAXP00010004

+   `MaxXMLNameLimit`: JAXP00010005

+   `maxElementDepth`: JAXP00010006

+   `EntityReplacementLimit`: JAXP00010007

错误代码的格式如下：

```java
"JAXP" + components (two digits) + error category (two digits) + sequence number

```

因此，代码 JAXP00010001 代表了 JAXP 基本解析器安全限制`EntityExpansionLimit`。
