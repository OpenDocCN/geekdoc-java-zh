# `try`-with-resources 语句

> 原文：[`docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html`](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

`try`-with-resources 语句是一个声明一个或多个资源的`try`语句。*资源*是程序在使用完后必须关闭的对象。`try`-with-resources 语句确保每个资源在语句结束时关闭。任何实现`java.lang.AutoCloseable`接口的对象，包括所有实现`java.io.Closeable`接口的对象，都可以用作资源。

以下示例从文件中读取第一行。它使用`FileReader`和`BufferedReader`的实例从文件中读取数据。`FileReader`和`BufferedReader`是在程序使用完后必须关闭的资源：

```java
	static String readFirstLineFromFile(String path) throws IOException {
	    try (FileReader fr = new FileReader(path);
	         BufferedReader br = new BufferedReader(fr)) {
	        return br.readLine();
	    }
	}	

```

在这个示例中，`try`-with-resources 语句中声明的资源是`FileReader`和`BufferedReader`。这些资源的声明语句出现在`try`关键字之后的括号内。在 Java SE 7 及更高版本中，`FileReader`和`BufferedReader`类实现了`java.lang.AutoCloseable`接口。因为`FileReader`和`BufferedReader`实例是在`try`-with-resources 语句中声明的，无论`try`语句是否正常完成或突然中断（因为`BufferedReader.readLine`方法抛出`IOException`），它们都将被关闭。

在 Java SE 7 之前，可以使用`finally`块来确保资源在`try`语句正常完成或突然中断时关闭。以下示例使用`finally`块而不是`try`-with-resources 语句：

```java
static String readFirstLineFromFileWithFinallyBlock(String path) throws IOException {

    FileReader fr = new FileReader(path);
    BufferedReader br = new BufferedReader(fr);
    try {
        return br.readLine();
    } finally {
        br.close();
        fr.close();
    }
}

```

然而，这个示例可能会有资源泄漏。程序不能仅仅依赖垃圾回收器（GC）在完成后回收资源的内存。程序还必须将资源释放回操作系统，通常通过调用资源的`close`方法。但是，如果程序在 GC 回收资源之前未能执行此操作，则释放资源所需的信息将丢失。这个资源，仍然被操作系统视为正在使用，已经泄漏。

在这个示例中，如果`readLine`方法抛出异常，并且`finally`块中的`br.close()`语句也抛出异常，那么`FileReader`就会泄漏。因此，使用`try`-with-resources 语句而不是`finally`块来关闭程序的资源。

如果`readLine`和`close`方法都抛出异常，则`readFirstLineFromFileWithFinallyBlock`方法会抛出从`finally`块中抛出的异常；从`try`块中抛出的异常会被抑制。相比之下，在示例`readFirstLineFromFile`中，如果`try`块和`try`-with-resources 语句都抛出异常，则`readFirstLineFromFile`方法会抛出从`try`块中抛出的异常；从`try`-with-resources 块中抛出的异常会被抑制。在 Java SE 7 及更高版本中，您可以检索被抑制的异常；有关更多信息，请参阅被抑制的异常部分。

以下示例检索打包在 zip 文件`zipFileName`中的文件的名称，并创建一个包含这些文件名称的文本文件：

```java
public static void writeToFileZipFileContents(String zipFileName,
                                           String outputFileName)
                                           throws java.io.IOException {

    java.nio.charset.Charset charset =
         java.nio.charset.StandardCharsets.US_ASCII;
    java.nio.file.Path outputFilePath =
         java.nio.file.Paths.get(outputFileName);

    // Open zip file and create output file with 
    // try-with-resources statement

    try (
        java.util.zip.ZipFile zf =
             new java.util.zip.ZipFile(zipFileName);
        java.io.BufferedWriter writer = 
            java.nio.file.Files.newBufferedWriter(outputFilePath, charset)
    ) {
        // Enumerate each entry
        for (java.util.Enumeration entries =
                                zf.entries(); entries.hasMoreElements();) {
            // Get the entry name and write it to the output file
            String newLine = System.getProperty("line.separator");
            String zipEntryName =
                 ((java.util.zip.ZipEntry)entries.nextElement()).getName() +
                 newLine;
            writer.write(zipEntryName, 0, zipEntryName.length());
        }
    }
}

```

在此示例中，`try`-with-resources 语句包含两个声明，它们之间用分号分隔：`ZipFile`和`BufferedWriter`。当直接跟在其后的代码块正常终止或因异常终止时，`BufferedWriter`和`ZipFile`对象的`close`方法会按照这个顺序自动调用。请注意，资源的`close`方法按照它们创建的*相反*顺序调用。

以下示例使用`try`-with-resources 语句自动关闭`java.sql.Statement`对象：

```java
public static void viewTable(Connection con) throws SQLException {

    String query = "select COF_NAME, SUP_ID, PRICE, SALES, TOTAL from COFFEES";

    try (Statement stmt = con.createStatement()) {
        ResultSet rs = stmt.executeQuery(query);

        while (rs.next()) {
            String coffeeName = rs.getString("COF_NAME");
            int supplierID = rs.getInt("SUP_ID");
            float price = rs.getFloat("PRICE");
            int sales = rs.getInt("SALES");
            int total = rs.getInt("TOTAL");

            System.out.println(coffeeName + ", " + supplierID + ", " + 
                               price + ", " + sales + ", " + total);
        }
    } catch (SQLException e) {
        JDBCTutorialUtilities.printSQLException(e);
    }
}

```

此示例中使用的资源`java.sql.Statement`是 JDBC 4.1 及更高版本 API 的一部分。

**注意**：`try`-with-resources 语句可以像普通的`try`语句一样具有`catch`和`finally`块。在`try`-with-resources 语句中，任何`catch`或`finally`块都会在声明的资源关闭后运行。

## 被抑制的异常

与`try`-with-resources 语句相关联的代码块中可能会抛出异常。在示例`writeToFileZipFileContents`中，异常可能会从`try`块中抛出，当尝试关闭`ZipFile`和`BufferedWriter`对象时，`try`-with-resources 语句最多可能会抛出两个异常。如果从`try`块中抛出异常，并且从`try`-with-resources 语句中抛出一个或多个异常，则这些从`try`-with-resources 语句中抛出的异常会被抑制，而由该块抛出的异常就是`writeToFileZipFileContents`方法抛出的异常。您可以通过调用由`try`块抛出的异常的`Throwable.getSuppressed`方法来检索这些被抑制的异常。

## 实现 AutoCloseable 或 Closeable 接口的类

查看[`AutoCloseable`](https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html)和[`Closeable`](https://docs.oracle.com/javase/8/docs/api/java/io/Closeable.html)接口的 Javadoc，以获取实现这两个接口之一的类列表。`Closeable`接口扩展了`AutoCloseable`接口。`Closeable`接口的`close`方法会抛出`IOException`类型的异常，而`AutoCloseable`接口的`close`方法会抛出`Exception`类型的异常。因此，`AutoCloseable`接口的子类可以重写`close`方法的行为，以抛出特定的异常，比如`IOException`，或者根本不抛出异常。
