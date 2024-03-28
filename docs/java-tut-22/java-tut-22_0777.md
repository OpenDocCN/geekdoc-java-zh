# 从任意数据结构生成 XML

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/xslt/generatingXML.html`](https://docs.oracle.com/javase/tutorial/jaxp/xslt/generatingXML.html)

本节使用 XSLT 将任意数据结构转换为 XML。

这是整个过程的概述：

1.  修改一个现有的读取数据的程序，使其生成 SAX 事件。（此时，该程序是一个真正的解析器还是一个数据过滤器都无关紧要）。

1.  使用 SAX“解析器”构建一个`SAXSource`进行转换。

1.  使用与上一个练习中创建的相同的`StreamResult`对象来显示结果。（但请注意，你也可以轻松地创建一个`DOMResult`对象来在内存中创建一个 DOM）。

1.  使用转换器对象将源连接到结果，进行转换。

首先，你需要一个要转换的数据集和一个能够读取数据的程序。接下来的两个部分创建了一个简单的数据文件和一个读取它的程序。

## 创建一个简单的文件

这个例子使用了一个地址簿的数据集，`PersonalAddressBook.ldif`。如果你还没有这样做，`下载 XSLT 示例`并解压到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录中。这里显示的文件是通过在 Netscape Messenger 中创建一个新的地址簿，给它一些虚拟数据（一个地址卡），然后以 LDAP 数据交换格式（LDIF）格式导出而生成的。解压 XSLT 示例后，它位于`xslt/data`目录中。

下面的图示显示了创建的通讯录条目。

图示 通讯录条目

![Mozilla Thunderbird 联系人详细信息卡的快照。](img/fc603efcd5e003bf0d8a41e181e83efc.png)

导出通讯录会生成一个类似下面显示的文件。我们关心的文件部分以粗体显示。

```java
dn: cn=Fred Flintstone,mail=fred@barneys.house
modifytimestamp: 20010409210816Z
cn: Fred Flintstone
xmozillanickname: Fred
mail: Fred@barneys.house
xmozillausehtmlmail: TRUE
givenname: Fred
sn: Flintstone
telephonenumber: 999-Quarry
homephone: 999-BedrockLane
facsimiletelephonenumber: 888-Squawk
pagerphone: 777-pager
cellphone: 555-cell
xmozillaanyphone: 999-Quarry
objectclass: top
objectclass: person

```

请注意，文件的每一行都包含一个变量名，一个冒号和一个空格，后面跟着变量的值。`sn`变量包含人的姓氏（姓），变量`cn`包含通讯录条目中的`DisplayName`字段。

## 创建一个简单的解析器

下一步是创建一个解析数据的程序。

* * *

**注意 -** 本节讨论的代码在`AddressBookReader01.java`中，解压`XSLT 示例`到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录后，可以在`xslt`目录中找到。

* * *

接下来显示程序的文本。这是一个非常简单的程序，甚至不会为多个条目循环，因为毕竟它只是一个演示。

```java
import java.io.*; 

public class AddressBookReader01 { 

    public static void main(String argv[]) {
        // Check the arguments
        if (argv.length != 1) {
            System.err.println("Usage: java AddressBookReader01 filename");
            System.exit (1);
        }

        String filename = argv[0];
        File f = new File(filename);
        AddressBookReader01 reader = new AddressBookReader01();
        reader.parse(f);
    }

    // Parse the input file
    public void parse(File f) {
        try {
            // Get an efficient reader for the file
            FileReader r = new FileReader(f);
            BufferedReader br = new BufferedReader(r);

            // Read the file and display its contents.
            String line = br.readLine();
            while (null != (line = br.readLine())) {
                if (line.startsWith("xmozillanickname: "))
                    break;
            }

            output("nickname", "xmozillanickname", line);
            line = br.readLine();
            output("email",  "mail", line);

            line = br.readLine();
            output("html", "xmozillausehtmlmail", line);

            line = br.readLine();
            output("firstname","givenname", line);

            line = br.readLine();
            output("lastname", "sn", line);

            line = br.readLine();
            output("work", "telephonenumber", line);

            line = br.readLine();
            output("home", "homephone", line);

            line = br.readLine();
            output("fax", "facsimiletelephonenumber", line);

            line = br.readLine();
            output("pager", "pagerphone", line);

            line = br.readLine();
            output("cell", "cellphone", line);
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

这个程序包含三个方法：

`main`

`main`方法从命令行获取文件名，创建解析器的实例，并开始解析文件。当我们将程序转换为 SAX 解析器时，这个方法将消失。（这是将解析代码放入单独方法的一个原因）。

`parse`

此方法在主例程发送的 `File` 对象上运行。正如您所看到的，这非常简单。唯一的效率让步是使用 `BufferedReader`，当您开始操作大文件时，这可能变得重要。

`output`

输出方法包含了一行结构的逻辑。它接受三个参数。第一个参数给出一个要显示的方法名称，因此它可以将 `html` 输出为一个变量名，而不是 `xmozillausehtmlmail`。第二个参数给出存储在文件中的变量名（`xmozillausehtmlmail`）。第三个参数给出包含数据的行。然后该例程从行的开头剥离变量名并输出所需的名称，加上数据。

### 运行 `AddressBookReader01` 示例

1.  **导航到 `samples` 目录。**

    ```java
    % cd *install-dir*/jaxp-1_4_2-*release-date*/samples.

    ```

1.  **点击此链接下载 XSLT 示例 并将其解压缩到 *install-dir*`/jaxp-1_4_2-`*release-date*`/samples` 目录中。**

1.  **导航到 `xslt` 目录。**

    ```java
    cd xslt

    ```

1.  **编译 `AddressBookReader01` 示例。**

    输入以下命令：

    ```java
    % javac AddressBookReader01.java

    ```

1.  **在数据文件上运行 `AddressBookReader01` 示例。**

    在下面的情况下，`AddressBookReader01` 在上面显示的文件 `PersonalAddressBook.ldif` 上运行，在解压缩示例包后在 `xslt/data` 目录中找到。

    ```java
    % java AddressBookReader01 data/PersonalAddressBook.ldif

    ```

    您将看到以下输出：

    ```java
    nickname: Fred
    email: Fred@barneys.house
    html: TRUE
    firstname: Fred
    lastname: Flintstone
    work: 999-Quarry
    home: 999-BedrockLane
    fax: 888-Squawk
    pager: 777-pager
    cell: 555-cell

    ```

    这比 创建简单文件 中显示的文件更易读。

## 创建生成 SAX 事件的解析器

本节展示了如何使解析器生成 SAX 事件，以便您可以将其用作 XSLT 转换中的 `SAXSource` 对象的基础。

* * *

**注意 -** 本节讨论的代码在 `AddressBookReader02.java` 中，该文件在您将 `XSLT examples` 解压缩到 *install-dir*`/jaxp-1_4_2-`*release-date*`/samples` 目录后的 `xslt` 目录中找到。`AddressBookReader02.java` 是从 `AddressBookReader01.java` 改编而来，因此这里只讨论两个示例之间的代码差异。

* * *

`AddressBookReader02` 需要以下未在 `AddressBookReader01` 中使用的突出显示的类。

```java
import java.io.*; 

import org.xml.sax.*;
import org.xml.sax.helpers.AttributesImpl;

```

该应用程序还扩展了 `XmlReader`。此更改将应用程序转换为生成适当的 SAX 事件的解析器。

```java
public class AddressBookReader02 implements XMLReader { /* ... */ }

```

与 `AddressBookReader01` 示例不同，此应用程序没有 `main` 方法。

以下全局变量将在本节后面使用：

```java
public class AddressBookReader02 implements XMLReader {
    ContentHandler handler;

    String nsu = "";  
    Attributes atts = new AttributesImpl();
    String rootElement = "addressbook";

    String indent = "\n ";

    // ...
}

```

SAX `ContentHandler` 是将获得解析器生成的 SAX 事件的对象。为了将应用程序转换为 `XmlReader`，应用程序定义了一个 `setContentHandler` 方法。处理程序变量将保存在调用 `setContentHandler` 时发送的对象的引用。

当解析器生成 SAX 元素事件时，它将需要提供命名空间和属性信息。因为这是一个简单的应用程序，它为这两者定义了空值。

应用程序还为数据结构（`addressbook`）定义了一个根元素，并设置了一个缩进字符串以提高输出的可读性。

此外，解析方法被修改为接受 `InputSource`（而不是 `File`）作为参数，并考虑了它可能生成的异常：

```java
public void parse(InputSource input) throws IOException, SAXException

```

现在，不再像在 `AddressBookReader01` 中那样创建新的 `FileReader` 实例，而是将读取器封装在 `InputSource` 对象中：

```java
try {
    java.io.Reader r = input.getCharacterStream();
    BufferedReader Br = new BufferedReader(r);
    // ...
}

```

* * *

**注意 -** 下一节将展示如何创建输入源对象，实际放入其中的将是一个缓冲读取器。但是 `AddressBookReader` 可能会被其他人在某个地方使用。这一步确保处理将是高效的，无论给定的读取器是什么。

* * *

下一步是修改解析方法以生成文档和根元素的 SAX 事件。以下突出显示的代码实现了这一点：

```java
public void parse(InputSource input) {
    try {
        // ...
        String line = br.readLine();
        while (null != (line = br.readLine())) {
            if (line.startsWith("xmozillanickname: ")) 
                break;
        }

        if (handler == null) {
            throw new SAXException("No content handler");
        }

        handler.startDocument(); 
        handler.startElement(nsu, rootElement, rootElement, atts);

        output("nickname", "xmozillanickname", line);
        // ...
        output("cell", "cellphone", line);

        handler.ignorableWhitespace("\n".toCharArray(), 
            0,  // start index
            1   // length
        ); 
        handler.endElement(nsu, rootElement, rootElement);
        handler.endDocument(); 
    }
    catch (Exception e) {
        // ...
    }
}

```

在这里，应用程序检查解析器是否正确配置了 `ContentHandler`。（对于这个应用程序，我们不关心其他任何内容）。然后生成文档和根元素的开始事件，并通过发送根元素的结束事件和文档的结束事件来完成。

此时有两个值得注意的地方：

+   `setDocumentLocator` 事件未发送，因为这是可选的。如果它很重要，那么该事件将在 `startDocument` 事件之前立即发送。

+   在根元素结束之前生成了一个 `ignorableWhitespace` 事件。这也是可选的，但它极大地提高了输出的可读性，很快就会看到。（在这种情况下，空白包括一个单独的换行符，它以与将字符发送到字符方法相同的方式发送：作为字符数组、起始索引和长度）。

现在生成了文档和根元素的 SAX 事件，下一步是修改输出方法以为每个数据项生成适当的元素事件。删除对 `System.out.println(name + ": " + text)` 的调用，并添加以下突出显示的代码来实现：

```java
void output(String name, String prefix, String line) 
    throws SAXException {

    int startIndex = 
    prefix.length() + 2;   // 2=length of ": "
    String text = line.substring(startIndex);

    int textLength = line.length() - startIndex;
    handler.ignorableWhitespace (indent.toCharArray(), 
        0,    // start index
        indent.length()
    );
    handler.startElement(nsu, name, name /*"qName"*/, atts);
    handler.characters(line.toCharArray(), 
        startIndex,
        textLength;
    );
    handler.endElement(nsu, name, name);
}

```

因为 `ContentHandler` 方法可以将 `SAXExceptions` 发送回解析器，所以解析器必须准备好处理它们。在这种情况下，不会有任何异常，所以如果出现任何异常，应用程序将简单地允许失败。

然后计算数据的长度，再次生成一些可忽略的空白以提高可读性。在这种情况下，只有一个数据级别，所以我们可以使用一个固定的缩进字符串。（如果数据结构更加结构化，我们将不得不根据数据的嵌套计算缩进空间）。

* * *

**注意 -** 缩进字符串对数据本身没有影响，但会使输出更容易阅读。如果没有该字符串，所有元素将被连接在一起：

```java
<addressbook>
<nickname>Fred</nickname>
<email>...

```

* * *

接下来，以下方法配置解析器，以接收其生成的事件的 `ContentHandler`：

```java
void output(String name, String prefix, String line)
    throws SAXException {
    //  ...
}

// Allow an application to register a content event handler.
public void setContentHandler(ContentHandler handler) {
    this.handler = handler;
}  

// Return the current content handler.
public ContentHandler getContentHandler() {
    return this.handler;
}

```

还必须实现几个其他方法以满足 `XmlReader` 接口的要求。为了这个练习，所有这些方法都生成了空方法。然而，一个生产应用程序需要实现错误处理程序方法以生成更健壮的应用程序。不过，在这个例子中，以下代码为它们生成了空方法：

```java
// Allow an application to register an error event handler.
public void setErrorHandler(ErrorHandler handler) { } 

// Return the current error handler.
public ErrorHandler getErrorHandler() { 
    return null; 
}

```

然后，以下代码为剩余的 `XmlReader` 接口生成了空方法。（其中大部分对于真实的 SAX 解析器是有价值的，但对于像这样的数据转换应用程序却没有太大影响）。

```java
// Parse an XML document from a system identifier (URI).
public void parse(String systemId) throws IOException, SAXException 
{ } 

// Return the current DTD handler.
public DTDHandler getDTDHandler() { return null; } 

// Return the current entity resolver.
public EntityResolver getEntityResolver() { return null; } 

// Allow an application to register an entity resolver.
public void setEntityResolver(EntityResolver resolver) { } 

// Allow an application to register a DTD event handler.
public void setDTDHandler(DTDHandler handler) { } 

// Look up the value of a property.
public Object getProperty(String name) { return null; } 

// Set the value of a property.
public void setProperty(String name, Object value) { }  

// Set the state of a feature.
public void setFeature(String name, boolean value) { } 

// Look up the value of a feature.
public boolean getFeature(String name) { return false; }

```

现在您有一个可以用来生成 SAX 事件的解析器。在下一节中，您将使用它来构造一个 SAX 源对象，从而将数据转换为 XML。

## 使用解析器作为 `SAXSource`

给定一个用作事件源的 SAX 解析器，您可以构造一个转换器来生成一个结果。在本节中，`TransformerApp` 将被更新以生成一个流输出结果，尽管它也可以轻松地生成一个 DOM 结果。

* * *

**注意 -** 注意：本节讨论的代码位于 `TransformationApp03.java` 中，在将 `XSLT 示例` 解压缩到 *install-dir*`/jaxp-1_4_2-`*release-date*`/samples` 目录后，可以在 `xslt` 目录中找到。

* * *

要开始，`TransformationApp03` 与 `TransformationApp02` 的不同之处在于构建 `SAXSource` 对象所需导入的类。这些类如下所示。在这一点上，DOM 类已不再需要，因此已被丢弃，尽管保留它们不会造成任何伤害。

```java
import org.xml.sax.SAXException; 
import org.xml.sax.SAXParseException; 
import org.xml.sax.ContentHandler;
import org.xml.sax.InputSource;

import javax.xml.transform.sax.SAXSource; 
import javax.xml.transform.stream.StreamResult;

```

接下来，应用程序不再创建 DOM `DocumentBuilderFactory` 实例，而是创建了一个 SAX 解析器，即 `AddressBookReader` 的实例：

```java
public class TransformationApp03 {
    static Document document;  
    public static void main(String argv[]) {
        // ...
        // Create the sax "parser".
        AddressBookReader saxReader = new AddressBookReader();

        try {
            File f = new File(argv[0]);
            // ...
        }
        // ...
    }
}

```

然后，以下突出显示的代码构造了一个 `SAXSource` 对象

```java
// Use a Transformer for output
// ...
Transformer transformer = tFactory.newTransformer();

// Use the parser as a SAX source for input
FileReader fr = new FileReader(f);
BufferedReader br = new BufferedReader(fr);
InputSource inputSource = new InputSource(br);
SAXSource source = new SAXSource(saxReader, inputSource);
StreamResult result = new StreamResult(System.out);
transformer.transform(source, result);

```

在这里，`TransformationApp03` 构造了一个缓冲读取器（如前所述），并将其封装在一个输入源对象中。然后创建了一个 `SAXSource` 对象，将读取器和 `InputSource` 对象传递给它，然后将其传递给转换器。

当应用程序运行时，转换器将自身配置为 SAX 解析器（`AddressBookReader`）的 `ContentHandler`，并告诉解析器对 `inputSource` 对象进行操作。解析器生成的事件然后传递给转换器，转换器执行适当的操作并将数据传递给结果对象。

最后，`TransformationApp03` 不会生成异常，因此在 `TransformationApp02` 中看到的异常处理代码不再存在。

### 运行 `TransformationApp03` 示例

1.  **导航到 `samples` 目录。**

    ```java
    % cd *install-dir*/jaxp-1_4_2-*release-date*/samples.

    ```

1.  **点击此链接下载 XSLT 示例 并将其解压缩到 *install-dir*`/jaxp-1_4_2-`*release-date*`/samples` 目录中。**

1.  **导航到 `xslt` 目录。**

    ```java
    cd xslt

    ```

1.  编译`TransformationApp03`示例。

    输入以下命令：

    ```java
    % javac TransformationApp03.java

    ```

1.  运行`TransformationApp03`示例，转换您希望转换为 XML 的数据文件。

    在下面的情况下，在您解压缩示例包后，在`xslt/data`目录中找到的`PersonalAddressBook.ldif`文件上运行`TransformationApp03`。

    ```java
    % java TransformationApp03 
      data/PersonalAddressBook.ldif

    ```

    您将看到以下输出：

    ```java
    <?xml version="1.0" encoding="UTF-8"?>
    <addressbook>
        <nickname>Fred</nickname>
        <email>Fred@barneys.house</email>
        <html>TRUE</html>
        <firstname>Fred</firstname>
        <lastname>Flintstone</lastname>
        <work>999-Quarry</work>
        <home>999-BedrockLane</home>
        <fax>888-Squawk</fax>
        <pager>777-pager</pager>
        <cell>555-cell</cell>
    </addressbook>

    ```

    正如您所看到的，LDIF 格式文件`PersonalAddressBook`已被转换为 XML！
