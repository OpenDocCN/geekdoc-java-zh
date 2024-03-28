# 错误处理

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/properties/error.html`](https://docs.oracle.com/javase/tutorial/jaxp/properties/error.html)

由于这些属性是当前版本的新功能，建议应用程序捕获适合接口的异常，例如，在以下示例中捕获 SAXException。在旧版本上，应用程序可能正常工作，例如，示例代码包含以下方法，用于检测是否使用支持新属性的 JDK 版本或 JAXP 实现运行示例：

```java
public boolean isNewPropertySupported() {
       try {
           SAXParserFactory spf = SAXParserFactory.newInstance();
           SAXParser parser = spf.newSAXParser();
           parser.setProperty("http://javax.xml.XMLConstants/property/accessExternalDTD", "file");
       } catch (ParserConfigurationException ex) {
           fail(ex.getMessage());
       } catch (SAXException ex) {
           String err = ex.getMessage();
           if (err.indexOf("Property 'http://javax.xml.XMLConstants/property/accessExternalDTD' is not recognized.") > -1)
           {
             //expected, jaxp 1.5 not supported
             return false;
           }
       }
       return true;
  }

```

如果由于新属性设置的限制而拒绝访问外部资源，则将以以下格式抛出异常并带有错误信息：

```java
[type of construct]: Failed to read [type of construct] "[name of the external resource]", because "[type of restriction]" access is not allowed due to restriction set by the [property name] property.

```

例如，如果由于限制只允许使用 http 协议而拒绝获取外部 DTD，如下所示：`parser.setProperty("http://javax.xml.XMLConstants/property/accessExternalDTD", "file");`，并且解析器解析包含对`"http://java.sun.com/dtd/properties.dtd"`的外部引用的 XML 文件，则错误消息将如下所示：

```java
External DTD: Failed to read external DTD ''http://java.sun.com/dtd/properties.dtd'', because ''http'' access is not allowed due to restriction set by the accessExternalDTD property.

```
