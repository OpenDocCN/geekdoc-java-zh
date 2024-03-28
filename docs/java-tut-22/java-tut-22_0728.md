# 使用 WebRowSet 对象

> [`docs.oracle.com/javase/tutorial/jdbc/basics/webrowset.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/webrowset.html)

`WebRowSet`对象非常特殊，因为除了提供`CachedRowSet`对象的所有功能外，它还可以将自身写入为 XML 文档，并且还可以读取该 XML 文档以将自身转换回`WebRowSet`对象。由于 XML 是异构企业之间可以相互通信的语言，因此它已成为 Web 服务通信的标准。因此，`WebRowSet`对象通过使 Web 服务能够以 XML 文档的形式从数据库发送和接收数据来填补了一个真正的需求。

下面涵盖了以下主题：

+   创建和填充 WebRowSet 对象

+   将 WebRowSet 对象写入和读取为 XML

+   XML 文档中的内容是什么

+   对 WebRowSet 对象进行更改

Coffee Break 公司已扩展到在线销售咖啡。用户可以从 Coffee Break 网站按磅订购咖啡。价格列表定期更新，通过从公司数据库获取最新信息。本节演示了如何通过`WebRowSet`对象和单个方法调用将价格数据发送为 XML 文档。

## 创建和填充 WebRowSet 对象

通过使用`RowSetFactory`的实例创建一个新的`WebRowSet`对象，该实例是从`RowSetProvider`类创建的，用于创建一个`WebRowSet`对象。以下示例来自`WebRowSetSample.java`：

```java
    RowSetFactory factory = RowSetProvider.newFactory();  
    try (WebRowSet priceList = factory.createWebRowSet();
         // ...
    ) {	  
      // ...

```

尽管`priceList`对象尚无数据，但它具有`BaseRowSet`对象的默认属性。其`SyncProvider`对象首先设置为`RIOptimisticProvider`实现，这是所有断开连接的`RowSet`对象的默认值。但是，`WebRowSet`实现会将`SyncProvider`对象重置为`RIXMLProvider`实现。

您可以使用`RowSetFactory`的实例创建一个`WebRowSet`对象，该实例是从`RowSetProvider`类创建的。有关更多信息，请参见使用 RowSetFactory 接口中的使用 JdbcRowSet 对象。

Coffee Break 总部定期向其网站发送价格列表更新。关于`WebRowSet`对象的这些信息将展示您可以通过 XML 文档发送最新价格列表的一种方式。

价格列表包括来自表`COFFEES`的列`COF_NAME`和`PRICE`中的数据。以下代码片段设置所需的属性，并使用价格列表数据填充`priceList`对象：

```java
      int[] keyCols = {1};
      priceList.setUsername(settings.userName);
      priceList.setPassword(settings.password);
      priceList.setUrl(settings.urlString);
      priceList.setCommand("select COF_NAME, PRICE from COFFEES");
      priceList.setKeyColumns(keyCols);

      // Populate the WebRowSet
      priceList.execute();

```

此时，除了默认属性之外，`priceList`对象还包含来自`COFFEES`表中`COF_NAME`和`PRICE`列的数据，以及关于这两列的元数据。

## 将 WebRowSet 对象写入和读取为 XML

要将`WebRowSet`对象写入 XML 文档，请调用方法`writeXml`。要将该 XML 文档的内容读入`WebRowSet`对象，请调用方法`readXml`。这两种方法都在后台执行其工作，除了结果之外，其他都对您不可见。

### 使用`writeXml`方法

方法`writeXml`将调用它的`WebRowSet`对象作为表示其当前状态的 XML 文档写入。它将这个 XML 文档写入您传递给它的流。流可以是一个`OutputStream`对象，比如一个`FileOutputStream`对象，或者一个`Writer`对象，比如一个`FileWriter`对象。如果您向方法`writeXml`传递一个`OutputStream`对象，您将以字节形式写入，可以处理所有类型的数据；如果您向它传递一个`Writer`对象，您将以字符形式写入。以下代码演示将`WebRowSet`对象`priceList`作为 XML 文档写入`FileOutputStream`对象`oStream`：

```java
java.io.FileOutputStream oStream =
    new java.io.FileOutputStream("priceList.xml");
priceList.writeXml(oStream);

```

以下代码将代表`priceList`的 XML 文档写入`FileWriter`对象`writer`，而不是写入`OutputStream`对象。`FileWriter`类是一个方便的用于向文件写入字符的类。

```java
java.io.FileWriter writer =
    new java.io.FileWriter("priceList.xml");
priceList.writeXml(writer);

```

方法`writeXml`的另外两个版本允许您在将其写入流之前，使用`ResultSet`对象的内容填充`WebRowSet`对象。在下面的代码行中，方法`writeXml`将`ResultSet`对象`rs`的内容读入`priceList`对象，然后将`priceList`作为 XML 文档写入`FileOutputStream`对象`oStream`：

```java
priceList.writeXml(rs, oStream);

```

在下一行代码中，`writeXml`方法将`priceList`填充为`rs`的内容，但将 XML 文档写入`FileWriter`对象，而不是写入`OutputStream`对象：

```java
priceList.writeXml(rs, writer);

```

### 使用`readXml`方法

方法`readXml`解析 XML 文档以构造 XML 文档描述的`WebRowSet`对象。与方法`writeXml`类似，您可以向`readXml`传递一个`InputStream`对象或`Reader`对象，从中读取 XML 文档。

```java
java.io.FileInputStream iStream =
    new java.io.FileInputStream("priceList.xml");
priceList.readXml(iStream);

java.io.FileReader reader = new
    java.io.FileReader("priceList.xml");
priceList.readXml(reader);

```

请注意，您可以将 XML 描述读入一个新的`WebRowSet`对象中，或者读入调用`writeXml`方法的相同`WebRowSet`对象中。在从总部发送价格列表信息到网站的情况下，您将使用一个新的`WebRowSet`对象，如下面的代码所示：

```java
WebRowSet recipient = new WebRowSetImpl();
java.io.FileReader reader =
    new java.io.FileReader("priceList.xml");
recipient.readXml(reader);

```

## XML 文档中包含什么

`RowSet`对象不仅包含它们所包含的数据，还包含有关其列的属性和元数据。因此，表示`WebRowSet`对象的 XML 文档除了数据外还包括其他信息。此外，XML 文档中的数据包括当前值和原始值。 （回想一下，原始值是在对数据进行最近更改之前立即存在的值。这些值对于检查数据库中的相应值是否已更改是必要的，从而创建关于应该持久化哪个值的冲突：您放入`RowSet`对象的新值还是其他人放入数据库中的新值。）

WebRowSet XML 模式本身是一个 XML 文档，定义了表示`WebRowSet`对象的 XML 文档将包含什么以及必须以什么格式呈现。发送方和接收方都使用此模式，因为它告诉发送方如何编写 XML 文档（表示`WebRowSet`对象的文档）以及接收方如何解析 XML 文档。由于实际的写入和读取是由`writeXml`和`readXml`方法的实现在内部完成的，因此作为用户，您不需要了解 WebRowSet XML 模式文档中的内容。

XML 文档以分层结构包含元素和子元素。以下是描述`WebRowSet`对象的 XML 文档中的三个主要元素：

+   属性

+   元数据

+   数据

元素标签表示元素的开始和结束。例如，`<properties>`标签表示属性元素的开始，`</properties>`标签表示其结束。`<map/>`标签是一种简写方式，表示尚未为地图子元素（属性元素中的一个子元素）分配值。以下示例 XML 文档使用间距和缩进使其更易于阅读，但在实际的 XML 文档中不使用这些，其中间距不表示任何内容。

接下来的三个部分向您展示了在示例`WebRowSetSample`中创建的`WebRowSet` `priceList`对象的三个主要元素包含什么。

### 属性

在`priceList`对象上调用`writeXml`方法将生成描述`priceList`的 XML 文档。此 XML 文档的属性部分将如下所示：

```java
<properties>
  <command>
    select COF_NAME, PRICE from COFFEES
  </command>
  <concurrency>1008</concurrency>
  <datasource><null/></datasource>
  <escape-processing>true</escape-processing>
  <fetch-direction>1000</fetch-direction>
  <fetch-size>0</fetch-size>
  <isolation-level>2</isolation-level>
  <key-columns>
    <column>1</column>
  </key-columns>
  <map>
  </map>
  <max-field-size>0</max-field-size>
  <max-rows>0</max-rows>
  <query-timeout>0</query-timeout>
  <read-only>true</read-only>
  <rowset-type>
    ResultSet.TYPE_SCROLL_INSENSITIVE
  </rowset-type>
  <show-deleted>false</show-deleted>
  <table-name>COFFEES</table-name>
  <url>jdbc:mysql://localhost:3306/testdb</url>
  <sync-provider>
    <sync-provider-name>
      com.sun.rowset.providers.RIOptimisticProvider
    </sync-provider-name>
    <sync-provider-vendor>
      Sun Microsystems Inc.
    </sync-provider-vendor>
    <sync-provider-version>
      1.0
    </sync-provider-version>
    <sync-provider-grade>
      2
    </sync-provider-grade>
    <data-source-lock>1</data-source-lock>
  </sync-provider>
</properties>

```

请注意，某些属性没有值。例如，`datasource`属性用`<datasource/>`标签表示，这是一种简写方式，表示`<datasource></datasource>`。没有给出值，因为已设置`url`属性。建立的任何连接将使用此 JDBC URL 完成，因此不需要设置`DataSource`对象。此外，`username`和`password`属性未列出，因为它们必须保持机密。

### 元数据

描述`WebRowSet`对象的 XML 文档的元数据部分包含有关该`WebRowSet`对象中列的信息。以下显示了描述`priceList`对象的`WebRowSet`对象的此部分的外观。因为`priceList`对象有两列，描述它的 XML 文档有两个`<column-definition>`元素。每个`<column-definition>`元素都有子元素提供有关所描述列的信息。

```java
<metadata>
  <column-count>2</column-count>
  <column-definition>
    <column-index>1</column-index>
    <auto-increment>false</auto-increment>
    <case-sensitive>false</case-sensitive>
    <currency>false</currency>
    <nullable>0</nullable>
    <signed>false</signed>
    <searchable>true</searchable>
    <column-display-size>
      32
    </column-display-size>
    <column-label>COF_NAME</column-label>
    <column-name>COF_NAME</column-name>
    <schema-name></schema-name>
    <column-precision>32</column-precision>
    <column-scale>0</column-scale>
    <table-name>coffees</table-name>
    <catalog-name>testdb</catalog-name>
    <column-type>12</column-type>
    <column-type-name>
      VARCHAR
    </column-type-name>
  </column-definition>
  <column-definition>
    <column-index>2</column-index>
    <auto-increment>false</auto-increment>
    <case-sensitive>true</case-sensitive>
    <currency>false</currency>
    <nullable>0</nullable>
    <signed>true</signed>
    <searchable>true</searchable>
    <column-display-size>
      12
    </column-display-size>
    <column-label>PRICE</column-label>
    <column-name>PRICE</column-name>
    <schema-name></schema-name>
    <column-precision>10</column-precision>
    <column-scale>2</column-scale>
    <table-name>coffees</table-name>
    <catalog-name>testdb</catalog-name>
    <column-type>3</column-type>
    <column-type-name>
      DECIMAL
    </column-type-name>
  </column-definition>
</metadata>

```

从这个元数据部分，你可以看到每行中有两列。第一列是`COF_NAME`，它保存`VARCHAR`类型的值。第二列是`PRICE`，它保存`REAL`类型的值，等等。请注意，列类型是数据源中使用的数据类型，而不是 Java 编程语言中的类型。要获取或更新`COF_NAME`列中的值，你可以使用`getString`或`updateString`方法，驱动程序会将其转换为`VARCHAR`类型，就像通常做的那样。

### 数据

数据部分提供了`WebRowSet`对象每行中每列的值。如果你已经填充了`priceList`对象并且没有对其进行任何更改，XML 文档的数据元素将如下所示。在下一节中，你将看到当你修改`priceList`对象中的数据时，XML 文档如何变化。

每一行都有一个`<currentRow>`元素，因为`priceList`有两列，所以每个`<currentRow>`元素包含两个`<columnValue>`元素。

```java
<data>
  <currentRow>
    <columnValue>Colombian</columnValue>
    <columnValue>7.99</columnValue>
  </currentRow>
  <currentRow>
    <columnValue>
      Colombian_Decaf
    </columnValue>
    <columnValue>8.99</columnValue>
  </currentRow>
  <currentRow>
    <columnValue>Espresso</columnValue>
    <columnValue>9.99</columnValue>
  </currentRow>
  <currentRow>
    <columnValue>French_Roast</columnValue>
    <columnValue>8.99</columnValue>
  </currentRow>
  <currentRow>
    <columnValue>French_Roast_Decaf</columnValue>
    <columnValue>9.99</columnValue>
  </currentRow>
</data>

```

## 对 WebRowSet 对象进行更改

对`WebRowSet`对象进行更改的方式与对`CachedRowSet`对象相同。然而，与`CachedRowSet`对象不同，`WebRowSet`对象会跟踪更新、插入和删除，以便`writeXml`方法可以写入当前值和原始值。接下来的三个部分演示了对数据进行更改，并展示了描述`WebRowSet`对象的 XML 文档在每次更改后的样子。关于 XML 文档，你无需做任何事情；任何对它的更改都是自动进行的，就像写入和读取 XML 文档一样。

### 插入行

如果 Coffee Break 连锁店的老板想要在价格表中添加一种新的咖啡，代码可能如下所示：

```java
priceList.absolute(3);
priceList.moveToInsertRow();
priceList.updateString(COF_NAME, "Kona");
priceList.updateFloat(PRICE, 8.99f);
priceList.insertRow();
priceList.moveToCurrentRow();

```

在参考实现中，插入会立即在当前行之后进行。在前面的代码片段中，当前行是第三行，因此新行将在第三行之后添加，并成为新的第四行。为了反映这个插入，XML 文档将在`<data>`元素中第三个`<currentRow>`元素之后添加以下`<insertRow>`元素。

`<insertRow>`元素看起来类似于以下内容。

```java
<insertRow>
  <columnValue>Kona</columnValue>
  <columnValue>8.99</columnValue>
</insertRow>

```

## 删除行

所有者认为浓缩咖啡销售不足，应从 The Coffee Break 商店出售的咖啡中移除。因此，所有者希望从价格表中删除浓缩咖啡。浓缩咖啡位于`priceList`对象的第三行，因此以下代码行将其删除：

```java
priceList.absolute(3); priceList.deleteRow();

```

以下`<deleteRow>`元素将出现在 XML 文档数据部分的第二行之后，表示第三行已被删除。

```java
<deleteRow>
  <columnValue>Espresso</columnValue>
  <columnValue>9.99</columnValue>
</deleteRow>

```

## 修改行

所有者进一步决定哥伦比亚咖啡的价格太贵，想将其降至每磅$6.99。以下代码设置了哥伦比亚咖啡的新价格，即第一行为每磅$6.99：

```java
priceList.first();
priceList.updateFloat(PRICE, 6.99);

```

XML 文档将在`<updateRow>`元素中反映这一变化，给出新值。第一列的值未更改，因此仅有第二列的`<updateValue>`元素：

```java
<currentRow>
  <columnValue>Colombian</columnValue>
  <columnValue>7.99</columnValue>
  <updateRow>6.99</updateRow>
</currentRow>

```

此时，通过插入一行、删除一行和修改一行，`priceList`对象的 XML 文档将如下所示：

```java
<data>
  <insertRow>
    <columnValue>Kona</columnValue>
    <columnValue>8.99</columnValue>
  </insertRow>
  <currentRow>
    <columnValue>Colombian</columnValue>
    <columnValue>7.99</columnValue>
    <updateRow>6.99</updateRow>
  </currentRow>
  <currentRow>
    <columnValue>
      Colombian_Decaf
    </columnValue>
    <columnValue>8.99</columnValue>
  </currentRow>
  <deleteRow>
    <columnValue>Espresso</columnValue>
    <columnValue>9.99</columnValue>
  </deleteRow>
  <currentRow>
    <columnValue>French_Roast</columnValue>
    <columnValue>8.99</columnValue>
  </currentRow>
  <currentRow>
    <columnValue>
      French_Roast_Decaf
    </columnValue>
    <columnValue>9.99</columnValue>
  </currentRow>
</data>

```

## WebRowSet 代码示例

示例`WebRowSetSample.java`展示了本页面描述的所有功能。
