# 使用 SQLXML 对象

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/sqlxml.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqlxml.html)

`Connection` 接口支持使用 `createSQLXML` 方法创建 `SQLXML` 对象。创建的对象不包含任何数据。可以通过在 `SQLXML` 接口上调用 `setString`、`setBinaryStream`、`setCharacterStream` 或 `setResult` 方法向对象添加数据。

下面涵盖了以下主题：

+   创建 SQLXML 对象

+   在 ResultSet 中检索 SQLXML 值

+   访问 SQLXML 对象数据

+   存储 SQLXML 对象

+   初始化 SQLXML 对象

+   释放 SQLXML 资源

+   示例代码

## 创建 SQLXML 对象

在以下摘录中，使用 `Connection.createSQLXML` 方法创建一个空的 `SQLXML` 对象。使用 `SQLXML.setString` 方法将数据写入创建的 `SQLXML` 对象。

```java
Connection con = DriverManager.getConnection(url, props);
SQLXML xmlVal = con.createSQLXML();
xmlVal.setString(val);

```

## 在 ResultSet 中检索 SQLXML 值

`SQLXML` 数据类型类似于更基本的内置类型。可以通过在 `ResultSet` 或 `CallableStatement` 接口中调用 `getSQLXML` 方法来检索 `SQLXML` 值。

例如，以下摘录从 `ResultSet` *rs* 的第一列检索一个 `SQLXML` 值：

```java
SQLXML xmlVar = rs.getSQLXML(1);

```

`SQLXML` 对象在创建它们的事务持续时间内至少保持有效，除非调用它们的 `free` 方法。

## 访问 SQLXML 对象数据

`SQLXML` 接口提供了 `getString`、`getBinaryStream`、`getCharacterStream` 和 `getSource` 方法来访问其内部内容。以下摘录使用 `getString` 方法检索 `SQLXML` 对象的内容：

```java
SQLXML xmlVal= rs.getSQLXML(1);
String val = xmlVal.getString();

```

可以使用 `getBinaryStream` 或 `getCharacterStream` 方法获取可直接传递给 XML 解析器的 `InputStream` 或 `Reader` 对象。以下摘录从 `SQLXML` 对象获取一个 `InputStream` 对象，然后使用 DOM（文档对象模型）解析器处理流：

```java
SQLXML sqlxml = rs.getSQLXML(column);
InputStream binaryStream = sqlxml.getBinaryStream();
DocumentBuilder parser = 
    DocumentBuilderFactory.newInstance().newDocumentBuilder();
Document result = parser.parse(binaryStream);

```

`getSource` 方法返回一个 `javax.xml.transform.Source` 对象。源用作 XML 解析器和 XSLT 转换器的输入。

以下摘录使用通过调用 `getSource` 方法返回的 `SAXSource` 对象从 `SQLXML` 对象中检索和解析数据：

```java
SQLXML xmlVal= rs.getSQLXML(1);
SAXSource saxSource = sqlxml.getSource(SAXSource.class);
XMLReader xmlReader = saxSource.getXMLReader();
xmlReader.setContentHandler(myHandler);
xmlReader.parse(saxSource.getInputSource());

```

## 存储 SQLXML 对象

`SQLXML` 对象可以像其他数据类型一样作为输入参数传递给 `PreparedStatement` 对象。`setSQLXML` 方法使用 `SQLXML` 对象设置指定的 `PreparedStatement` 参数。

在以下摘录中，`authorData` 是一个 `java.sql.SQLXML` 接口的实例，其数据先前已初始化。

```java
PreparedStatement pstmt = conn.prepareStatement("INSERT INTO bio " +
                              "(xmlData, authId) VALUES (?, ?)");
pstmt.setSQLXML(1, authorData);
pstmt.setInt(2, authorId);

```

`updateSQLXML` 方法可用于更新可更新结果集中的列值。

如果在调用`setSQLXML`或`updateSQLXML`之前未关闭`SQLXML`对象的`java.xml.transform.Result`、`Writer`或`OutputStream`对象，将抛出`SQLException`。

## 初始化 SQLXML 对象

`SQLXML`接口提供了`setString`、`setBinaryStream`、`setCharacterStream`或`setResult`方法来初始化通过调用`Connection.createSQLXML`方法创建的`SQLXML`对象的内容。

以下摘录使用`setResult`方法返回一个`SAXResult`对象以填充一个新创建的`SQLXML`对象：

```java
SQLXML sqlxml = con.createSQLXML();
SAXResult saxResult = sqlxml.setResult(SAXResult.class);
ContentHandler contentHandler = saxResult.getXMLReader().getContentHandler();
contentHandler.startDocument();

// set the XML elements and
// attributes into the result
contentHandler.endDocument();

```

以下摘录使用`setCharacterStream`方法获取一个`java.io.Writer`对象以初始化一个`SQLXML`对象：

```java
SQLXML sqlxml = con.createSQLXML();
Writer out= sqlxml.setCharacterStream();
BufferedReader in = new BufferedReader(new FileReader("xml/foo.xml"));
String line = null;
while((line = in.readLine() != null) {
    out.write(line);
}

```

同样地，`SQLXML`的`setString`方法可用于初始化一个`SQLXML`对象。

如果尝试在先前初始化过的`SQLXML`对象上调用`setString`、`setBinaryStream`、`setCharacterStream`和`setResult`方法，将抛出`SQLException`。如果对同一`SQLXML`对象多次调用`setBinaryStream`、`setCharacterStream`和`setResult`方法，则会抛出`SQLException`，并且先前返回的`javax.xml.transform.Result`、`Writer`或`OutputStream`对象不受影响。

## 释放 SQLXML 资源

`SQLXML`对象在创建它们的事务持续时间内至少保持有效。这可能导致应用程序在长时间运行的事务中耗尽资源。应用程序可以通过调用它们的`free`方法释放`SQLXML`资源。

在以下摘录中，调用`method SQLXML.free`来释放先前创建的`SQLXML`对象所持有的资源。

```java
SQLXML xmlVar = con.createSQLXML();
xmlVar.setString(val);
xmlVar.free();

```

## 示例代码

MySQL 和 Java DB 及其各自的 JDBC 驱动程序并不完全支持本节中描述的`SQLXML` JDBC 数据类型。然而，示例`RSSFeedsTable.java`演示了如何处理 MySQL 和 Java DB 中的 XML 数据。

The Coffee Break 的所有者关注来自各种网站的几个 RSS 订阅源，涵盖餐厅和饮料行业新闻。RSS（真正简单的聚合或富站点摘要）订阅源是一个包含一系列文章和相关元数据的 XML 文档，如每篇文章的发布日期和作者。所有者希望将这些 RSS 订阅源存储到数据库表中，包括 The Coffee Break 博客的 RSS 订阅源。

文件`rss-the-coffee-break-blog.xml`是 The Coffee Break 博客的一个示例 RSS 订阅源。文件`rss-coffee-industry-news.xml`是（虚构的）Coffee Industry News 的一个示例 RSS 订阅源。

### 在 MySQL 中处理 XML 数据

示例`RSSFeedsTable`将 RSS 订阅源存储在名为`RSS_FEEDS`的表中，该表是使用以下命令创建的：

```java
create table RSS_FEEDS
    (RSS_NAME varchar(32) NOT NULL,
    RSS_FEED_XML longtext NOT NULL,
    PRIMARY KEY (RSS_NAME));

```

MySQL 不支持 XML 数据类型。相反，此示例将 XML 数据存储在类型为 `LONGTEXT` 的列中，这是一种 `CLOB` SQL 数据类型。MySQL 有四种 `CLOB` 数据类型；`LONGTEXT` 数据类型在这四种类型中包含的字符数量最多。

方法 `RSSFeedsTable.addRSSFeed` 将一个 RSS 订阅添加到 `RSS_FEEDS` 表中。此方法的第一条语句将 RSS 订阅（在此示例中表示为 XML 文件）转换为类型为 `org.w3c.dom.Document` 的对象，该对象表示 DOM（文档对象模型）文档。这个类以及包含在 `javax.xml` 包中的类和接口包含了使您能够操作 XML 数据内容的方法。例如，以下语句使用 XPath 表达式从 `Document` 对象中检索 RSS 订阅的标题：

```java
Node titleElement =
    (Node)xPath.evaluate("/rss/channel/title[1]",
        doc, XPathConstants.NODE);

```

XPath 表达式 `/rss/channel/title[1]` 检索第一个 `<title>` 元素的内容。对于文件 `rss-the-coffee-break-blog.xml`，这是字符串 `The Coffee Break Blog`。

以下语句将 RSS 订阅添加到表 `RSS_FEEDS` 中：

```java
// For databases that support the SQLXML
// data type, this creates a
// SQLXML object from
// org.w3c.dom.Document.

System.out.println("Adding XML file " + fileName);
String insertRowQuery =
    "insert into RSS_FEEDS " +
    "(RSS_NAME, RSS_FEED_XML) values " +
    "(?, ?)";
insertRow = con.prepareStatement(insertRowQuery);
insertRow.setString(1, titleString);

System.out.println("Creating SQLXML object with MySQL");
rssData = con.createSQLXML();
System.out.println("Creating DOMResult object");
DOMResult dom = (DOMResult)rssData.setResult(DOMResult.class);
dom.setNode(doc);

insertRow.setSQLXML(2, rssData);
System.out.println("Running executeUpdate()");
insertRow.executeUpdate();

```

方法 `RSSFeedsTable.viewTable` 检索 `RSS_FEEDS` 的内容。对于每一行，该方法创建一个名为 `doc` 的类型为 `org.w3c.dom.Document` 的对象，用于存储列 `RSS_FEED_XML` 中的 XML 内容。该方法检索 XML 内容并将其存储在名为 `rssFeedXML` 的类型为 `SQLXML` 的对象中。`rssFeedXML` 的内容被解析并存储在 `doc` 对象中。

### 在 Java DB 中处理 XML 数据

**注意**：有关在 Java DB 中处理 XML 数据的更多信息，请参阅 [*Java DB 开发人员指南*](https://docs.oracle.com/javadb/index_jdk8.html) 中的 "XML 数据类型和运算符" 部分。

示例 `RSSFeedsTable` 将 RSS 订阅存储在表 `RSS_FEEDS` 中，该表是使用以下命令创建的：

```java
create table RSS_FEEDS
    (RSS_NAME varchar(32) NOT NULL,
    RSS_FEED_XML xml NOT NULL,
    PRIMARY KEY (RSS_NAME));

```

Java DB 支持 XML 数据类型，但不支持 `SQLXML` JDBC 数据类型。因此，您必须将任何 XML 数据转换为字符格式，然后使用 Java DB 运算符 `XMLPARSE` 将其转换为 XML 数据类型。

方法 `RSSFeedsTable.addRSSFeed` 将一个 RSS 订阅添加到 `RSS_FEEDS` 表中。此方法的第一条语句将 RSS 订阅（在此示例中表示为 XML 文件）转换为类型为 `org.w3c.dom.Document` 的对象。这在 在 MySQL 中处理 XML 数据 部分中有描述。

`RSSFeedsTable.addRSSFeed` 方法使用方法 `JDBCTutorialUtilities.convertDocumentToString` 将 RSS 订阅转换为 `String` 对象。

Java DB 有一个名为`XMLPARSE`的操作符，将字符字符串表示解析为 Java DB XML 值，以下摘录演示了这一点：

```java
String insertRowQuery =
    "insert into RSS_FEEDS " +
    "(RSS_NAME, RSS_FEED_XML) values " +
    "(?, xmlparse(document cast " +
    "(? as clob) preserve whitespace))";

```

`XMLPARSE`操作符要求您将 XML 文档的字符表示转换为 Java DB 识别的字符串数据类型。在本例中，它将其转换为`CLOB`数据类型。有关 Apache Xalan 和 Java DB 要求的更多信息，请参阅入门指南和 Java DB 文档。

方法`RSSFeedsTable.viewTable`检索`RSS_FEEDS`的内容。因为 Java DB 不支持 JDBC 数据类型`SQLXML`，您必须将 XML 内容检索为字符串。Java DB 有一个名为`XMLSERIALIZE`的操作符，将 XML 类型转换为字符类型：

```java
String query =
    "select RSS_NAME, " +
    "xmlserialize " +
    "(RSS_FEED_XML as clob) " +
    "from RSS_FEEDS";

```

与`XMLPARSE`操作符一样，`XMLSERIALIZE`操作符要求在您的 Java 类路径中列出 Apache Xalan。
