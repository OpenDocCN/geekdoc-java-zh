# 使用大对象

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/blob.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/blob.html)

`Blob`、`Clob`和`NClob` Java 对象的一个重要特性是，您可以在不将所有数据从数据库服务器传输到客户端计算机的情况下对它们进行操作。一些实现使用定位器（逻辑指针）来表示这些类型的实例，指向实例所代表的数据库中的对象。由于`BLOB`、`CLOB`或`NCLOB` SQL 对象可能非常大，使用定位器可以显著提高性能。但是，其他实现会在客户端计算机上完全实现大对象。

如果要将`BLOB`、`CLOB`或`NCLOB` SQL 值的数据传输到客户端计算机，请使用为此目的提供的`Blob`、`Clob`和`NClob` Java 接口中的方法。这些大对象类型对象将它们所代表的对象的数据实现为流。

以下主题涵盖：

+   向数据库添加大对象类型对象

+   检索 CLOB 值

+   添加和检索 BLOB 对象

+   释放大对象占用的资源

## 向数据库添加大对象类型对象

以下摘录自`ClobSample.addRowToCoffeeDescriptions`向`COFFEE_DESCRIPTIONS`表中添加`CLOB` SQL 值。`Clob` Java 对象`myClob`包含由`fileName`指定的文件的内容。

```java
  public void addRowToCoffeeDescriptions(String coffeeName,
                                         String fileName) throws SQLException {
    String sql = "INSERT INTO COFFEE_DESCRIPTIONS VALUES(?,?)";
    Clob myClob = this.con.createClob();
    try (PreparedStatement pstmt = this.con.prepareStatement(sql);
      Writer clobWriter = myClob.setCharacterStream(1);){
      String str = this.readFile(fileName, clobWriter);
      System.out.println("Wrote the following: " + clobWriter.toString());
      if (this.settings.dbms.equals("mysql")) {
        System.out.println("MySQL, setting String in Clob object with setString method");
        myClob.setString(1, str);
      }
      System.out.println("Length of Clob: " + myClob.length());
      pstmt.setString(1, coffeeName);
      pstmt.setClob(2, myClob);
      pstmt.executeUpdate();
    } catch (SQLException sqlex) {
      JDBCTutorialUtilities.printSQLException(sqlex);
    } catch (Exception ex) {
      System.out.println("Unexpected exception: " + ex.toString());
    }
  }

```

下一行创建一个`Clob` Java 对象：

```java
Clob myClob = this.con.createClob();

```

下一行检索一个流（在本例中为名为`clobWriter`的`Writer`对象），用于将一系列字符写入`Clob` Java 对象`myClob`。`ClobSample.readFile`方法写入这些字符流；流来自由`String` `fileName`指定的文件。方法参数`1`表示`Writer`对象将从`Clob`值的开头开始写入字符流：

```java
Writer clobWriter = myClob.setCharacterStream(1);

```

`ClobSample.readFile`方法逐行读取由文件`fileName`指定的文件，并将其写入由`writerArg`指定的`Writer`对象：

```java
  private String readFile(String fileName, Writer writerArg) throws IOException {
    try (BufferedReader br = new BufferedReader(new FileReader(fileName))) {
      String nextLine = "";
      StringBuffer sb = new StringBuffer();
      while ((nextLine = br.readLine()) != null) {
        System.out.println("Writing: " + nextLine);
        writerArg.write(nextLine);
        sb.append(nextLine);
      }
      // Convert the content into to a string
      String clobData = sb.toString();
	  // Return the data.
      return clobData;
	}
  }

```

以下摘录创建一个`PreparedStatement`对象`pstmt`，将`Clob` Java 对象`myClob`插入`COFFEE_DESCRIPTIONS`中：

```java
    String sql = "INSERT INTO COFFEE_DESCRIPTIONS VALUES(?,?)";
    Clob myClob = this.con.createClob();
    try (PreparedStatement pstmt = this.con.prepareStatement(sql);
      // ...
      ) {
      // ...
      pstmt.setString(1, coffeeName);
      pstmt.setClob(2, myClob);
      pstmt.executeUpdate();
      // ...

```

## 检索 CLOB 值

方法`ClobSample.retrieveExcerpt`从`COFFEE_DESCRIPTIONS`表中`COF_NAME`列的值等于`coffeeName`参数指定的`String`值的行中检索存储在`COF_DESC`列中的`CLOB` SQL 值：

```java
  public String retrieveExcerpt(String coffeeName,
                                int numChar) throws SQLException {

    String description = null;
    Clob myClob = null;
    String sql = "select COF_DESC from COFFEE_DESCRIPTIONS where COF_NAME = ?";

    try (PreparedStatement pstmt = this.con.prepareStatement(sql)) {
      pstmt.setString(1, coffeeName);
      ResultSet rs = pstmt.executeQuery();
      if (rs.next()) {
        myClob = rs.getClob(1);
        System.out.println("Length of retrieved Clob: " + myClob.length());
      }
      description = myClob.getSubString(1, numChar);
    } catch (SQLException sqlex) {
      JDBCTutorialUtilities.printSQLException(sqlex);
    } catch (Exception ex) {
      System.out.println("Unexpected exception: " + ex.toString());
    }
    return description;
  }

```

下一行从`ResultSet`对象`rs`中检索`Clob` Java 值：

```java
myClob = rs.getClob(1);

```

以下行从`myClob`对象中检索子字符串。子字符串从`myClob`值的第一个字符开始，最多有`numChar`指定的连续字符数，其中`numChar`是一个整数。

```java
description = myClob.getSubString(1, numChar);

```

## 添加和检索 BLOB 对象

添加和检索`BLOB` SQL 对象类似于添加和检索`CLOB` SQL 对象。使用`Blob.setBinaryStream`方法检索一个`OutputStream`对象，以写入调用该方法的`Blob` Java 对象（表示`BLOB` SQL 值）的`BLOB` SQL 值。

## 释放大对象所持有的资源

`Blob`、`Clob`和`NClob` Java 对象在它们被创建的事务持续时间内至少保持有效。这可能导致应用程序在长时间运行的事务中耗尽资源。应用程序可以通过调用它们的`free`方法来释放`Blob`、`Clob`和`NClob`资源。

在以下摘录中，调用方法`Clob.free`来释放先前创建的`Clob`对象所持有的资源：

```java
Clob aClob = con.createClob();
int numWritten = aClob.setString(1, val);
aClob.free();

```
