# 使用 Datalink 对象

> 原文：[`docs.oracle.com/javase/tutorial/jdbc/basics/sqldatalink.html`](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqldatalink.html)

`DATALINK`值通过 URL 引用底层数据源之外的资源。URL，统一资源定位符，是指向万维网上资源的指针。资源可以是简单的文件或目录，也可以是对更复杂对象的引用，比如对数据库查询或搜索引擎的查询。

下列主题包括：

+   存储外部数据引用

+   检索外部数据引用

## 存储外部数据引用

使用方法`PreparedStatement.setURL`来为预编译语句指定一个`java.net.URL`对象。在 URL 类型不被 Java 平台支持的情况下，可以使用`setString`方法存储 URL。

例如，假设 The Coffee Break 的所有者想要在数据库表中存储一组重要的 URL。以下方法`DatalinkSample.addURLRow`向表`DATA_REPOSITORY`添加一行数据。该行包括标识 URL 的字符串，`DOCUMENT_NAME`和 URL 本身，`URL`：

```java
  public void addURLRow(String description, String url) throws SQLException {
    String query = "INSERT INTO data_repository(document_name,url) VALUES (?,?)";
    try (PreparedStatement pstmt = this.con.prepareStatement(query)) {
      pstmt.setString(1, description);
      pstmt.setURL(2,new URL(url));
      pstmt.execute();    
    } catch (SQLException sqlex) {
      JDBCTutorialUtilities.printSQLException(sqlex);
    } catch (Exception ex) {
      System.out.println("Unexpected exception");
      ex.printStackTrace();
    }
  }

```

## 检索外部数据引用

使用方法`ResultSet.getURL`检索外部数据引用作为`java.net.URL`对象。在`getObject`或`getURL`方法返回的 URL 类型不被 Java 平台支持的情况下，通过调用`getString`方法将 URL 作为`String`对象检索。

下列方法`DatalinkSample.viewTable`显示了表`DATA_REPOSITORY`中所有 URL 的内容：

```java
  public static void viewTable(Connection con, Proxy proxy)
    throws SQLException, IOException {
    String query = "SELECT document_name, url FROM data_repository";
    try (Statement stmt = con.createStatement()) {
      ResultSet rs = stmt.executeQuery(query);
      if ( rs.next() )  {
        String documentName = null;
        java.net.URL url = null;
        documentName = rs.getString(1);
        // Retrieve the value as a URL object.
        url = rs.getURL(2);    
        if (url != null) {
          // Retrieve the contents from the URL.
          URLConnection myURLConnection = url.openConnection(proxy);
          BufferedReader bReader =
            new BufferedReader(new InputStreamReader(myURLConnection.getInputStream()));
          System.out.println("Document name: " + documentName);
          String pageContent = null;
          while ((pageContent = bReader.readLine()) != null ) {
            // Print the URL contents
            System.out.println(pageContent);
          }
        } else { 
          System.out.println("URL is null");
        } 
      }
    } catch (SQLException e) {
      JDBCTutorialUtilities.printSQLException(e);
    } catch(IOException ioEx) {
      System.out.println("IOException caught: " + ioEx.toString());
    } catch (Exception ex) {
      System.out.println("Unexpected exception");
      ex.printStackTrace();
    }
  }

```

示例`DatalinkSample.java`将 Oracle URL [`www.oracle.com`](https://www.oracle.com) 存储在表`DATA_REPOSITORY`中。然后，它显示了所有在`DATA_REPOSITORY`中存储的 URL 引用的文档内容，其中包括 Oracle 主页，[`www.oracle.com`](https://www.oracle.com)。

以下语句从结果集中检索 URL 作为`java.net.URL`对象：

```java
url = rs.getURL(2);

```

示例使用以下语句访问`URL`对象引用的数据：

```java
          // Retrieve the contents from the URL.
          URLConnection myURLConnection = url.openConnection(proxy);
          BufferedReader bReader =
            new BufferedReader(new InputStreamReader(myURLConnection.getInputStream()));
          System.out.println("Document name: " + documentName);
          String pageContent = null;
          while ((pageContent = bReader.readLine()) != null ) {
            // Print the URL contents
            System.out.println(pageContent);
          }

```

方法`URLConnection.openConnection`可以不带参数，这意味着`URLConnection`表示直接连接到互联网。如果需要代理服务器连接到互联网，`openConnection`方法接受一个`java.net.Proxy`对象作为参数。以下语句演示如何创建一个 HTTP 代理，服务器名称为`www-proxy.example.com`，端口号为`80`：

```java
Proxy myProxy;
InetSocketAddress myProxyServer;
myProxyServer = new InetSocketAddress("www-proxy.example.com", 80);
myProxy = new Proxy(Proxy.Type.HTTP, myProxyServer);

```
