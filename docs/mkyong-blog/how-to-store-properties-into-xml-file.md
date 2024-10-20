# Java–将属性文件转换为 XML

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-store-properties-into-xml-file/>

在 Java 中，我们可以使用`Properties#storeToXML()`将属性值转换成 XML 文件。

目录

*   [1。将属性值转换为 XML 文件](#convert-properties-values-to-xml-file)
*   [2。将属性文件转换成 XML 文件](#convert-a-properties-file-to-an-xml-file)
*   [3。下载源代码](#download-source-code)
*   [4。参考文献](#references)

用 Java 11 测试。

## 1。将属性值转换为 XML 文件

以下示例创建了一些属性值，并将它们存储为 XML 文件。

PropertiesToXml.java

```java
 package com.mkyong.xml.tips;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.nio.charset.StandardCharsets;
import java.util.Properties;

public class PropertiesToXml {

  public static void main(String[] args) throws IOException {

      // create some properties values on demand
      Properties props = new Properties();
      props.setProperty("email.support", "donot-spam-me@nospam.com");
      props.setProperty("http.port", "8080");
      props.setProperty("http.server", "localhost");

      try (OutputStream output =
          new FileOutputStream("c:\\test\\server-config.xml")) {

          // convert the properties to an XML file
          props.storeToXML(output, "Server config file", StandardCharsets.UTF_8);

      }

  }

} 
```

输出

c:\\test\\server-config.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
<comment>Server config file</comment>
<entry key="http.port">8080</entry>
<entry key="email.support">donot-spam-me@nospam.com</entry>
<entry key="http.server">localhost</entry>
</properties> 
```

## 2。将属性文件转换成 XML 文件

下面的例子加载了一个`.properties`文件并将它们存储为一个 XML 文件。

application.properties

```java
 greeting.message=hello
quarkus.http.port=8080 
```

PropertiesToXml2.java

```java
 package com.mkyong.xml.tips;

import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.Properties;

public class PropertiesToXml2 {

  public static void main(String[] args) throws IOException {

      Properties props = new Properties();
      try (InputStream input =
            new FileInputStream("src/main/resources/application.properties")) {
          // loads a properties file
          props.load(input);
      }

      try (OutputStream output =
            new FileOutputStream("c:\\test\\server-config.xml")) {

          // convert the properties to an XML file
          props.storeToXML(output, "Server config file",
                  StandardCharsets.UTF_8);

      }

  }

} 
```

输出

c:\\test\\server-config.xml

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
<comment>Server config file</comment>
<entry key="greeting.message">hello</entry>
<entry key="quarkus.http.port">8080</entry>
</properties> 
```

## 3。下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220626054143/https://github.com/mkyong/core-java)

$ cd java-xml

$ CD src/main/Java/com/mkyong/XML/tips/

## 4。参考文献

*   [属性 JavaDoc](http://web.archive.org/web/20220626054143/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Properties.html)
*   [Java 属性文件示例](http://web.archive.org/web/20220626054143/https://mkyong.com/java/java-properties-file-examples/)
*   [Java–将 XML 转换成属性文件](http://web.archive.org/web/20220626054143/https://mkyong.com/java/how-to-load-properties-from-xml-file/)

<input type="hidden" id="mkyong-current-postId" value="6855">