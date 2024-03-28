# 使用 SAX 解析 XML 文件

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/sax/parsing.html`](https://docs.oracle.com/javase/tutorial/jaxp/sax/parsing.html)

在实际应用中，您将希望使用 SAX 解析器处理 XML 数据并对其进行有用的操作。本节将介绍一个名为`SAXLocalNameCount`的 JAXP 程序示例，该程序仅使用元素的`localName`组件计算元素的数量，而忽略了命名空间名称以简化操作。此示例还展示了如何使用 SAX `ErrorHandler`。

## 创建骨架

`SAXLocalNameCount`程序保存在名为`SAXLocalNameCount.java`的文件中。

```java
public class SAXLocalNameCount {
    static public void main(String[] args) {
        // ...
    }
}

```

因为您将独立运行它，所以需要一个`main()`方法。并且您需要命令行参数，以便告诉应用程序要处理哪个文件。在`SAXLocalNameCount.java`文件中找到示例的完整代码。

## 导入类

应用程序将使用的类的导入语句如下。

```java
package sax;
import javax.xml.parsers.*;
import org.xml.sax.*;
import org.xml.sax.helpers.*;

import java.util.*;
import java.io.*;

public class SAXLocalNameCount {
    // ...
}

```

`javax.xml.parsers`包含`SAXParserFactory`类，用于创建所使用的解析器实例。如果无法生成与指定选项配置匹配的解析器，则会抛出`ParserConfigurationException`异常。（稍后，您将看到更多有关配置选项的内容）。`javax.xml.parsers`包还包含`SAXParser`类，这是工厂用于解析的返回值。`org.xml.sax`包定义了 SAX 解析器使用的所有接口。`org.xml.sax.helpers`包含`DefaultHandler`，它定义了将处理解析器生成的 SAX 事件的类。`java.util`和`java.io`中的类用于提供哈希表和输出。

## 设置 I/O

首要任务是处理命令行参数，目前这些参数仅用于获取要处理的文件的名称。`main`方法中的以下代码告诉应用程序您希望`SAXLocalNameCount`处理哪个文件。

```java
static public void main(String[] args) throws Exception {
    String filename = null;

    for (int i = 0; i < args.length; i++) {
        filename = args[i];
        if (i != args.length - 1) {
            usage();
        }
    }

    if (filename == null) {
        usage();
    } 
}

```

这段代码将`main`方法设置为在遇到问题时抛出`Exception`，并定义了命令行选项，这些选项是告诉应用程序要处理的 XML 文件的名称。在本课程的后面部分，当我们开始查看验证时，代码中的其他命令行参数将被检查。

当您运行应用程序时提供的`filename`字符串将通过内部方法`convertToFileURL()`转换为`java.io.File` URL。这是在`SAXLocalNameCount`中的以下代码完成的。

```java
public class SAXLocalNameCount {
    private static String convertToFileURL(String filename) {
        String path = new File(filename).getAbsolutePath();
        if (File.separatorChar != '/') {
            path = path.replace(File.separatorChar, '/');
        }

        if (!path.startsWith("/")) {
            path = "/" + path;
        }
        return "file:" + path;
    }

    // ...
}

```

如果在运行程序时指定了不正确的命令行参数，则会调用`SAXLocalNameCount`应用程序的`usage()`方法，以在屏幕上打印出正确的选项。

```java
private static void usage() {
    System.err.println("Usage: SAXLocalNameCount <file.xml>");
    System.err.println("       -usage or -help = this message");
    System.exit(1);
}

```

更多`usage()`选项将在本课程的后面部分进行讨论，当处理验证时。

## 实现`ContentHandler`接口

`SAXLocalNameCount`中最重要的接口是`ContentHandler`。该接口需要一些方法，SAX 解析器会在不同的解析事件发生时调用这些方法。主要的事件处理方法包括：`startDocument`、`endDocument`、`startElement`和`endElement`。

实现此接口的最简单方法是扩展`org.xml.sax.helpers`包中定义的`DefaultHandler`类。该类为所有`ContentHandler`事件提供了空操作方法。示例程序扩展了该类。

```java
public class SAXLocalNameCount extends DefaultHandler {
    // ...
} 

```

* * *

**注意 -** `DefaultHandler`还为`DTDHandler`、`EntityResolver`和`ErrorHandler`接口中定义的其他主要事件定义了空操作方法。您将在本课程的后续部分了解更多关于这些方法的信息。

* * *

接口要求这些方法中的每一个都抛出`SAXException`。在此抛出的异常会被发送回解析器，解析器会将其发送到调用解析器的代码。

## 处理内容事件

本节展示了处理`ContentHandler`事件的代码。

当遇到开始标签或结束标签时，标签的名称作为字符串传递给`startElement`或`endElement`方法。当遇到开始标签时，它定义的任何属性也会作为`Attributes`列表传递。元素中找到的字符会作为字符数组传递，同时还会传递字符的数量（长度）和指向第一个字符的数组偏移量。

### 文档事件

以下代码处理了开始文档和结束文档事件：

```java
public class SAXLocalNameCount extends DefaultHandler {

    private Hashtable tags;

    public void startDocument() throws SAXException {
        tags = new Hashtable();
    }

    public void endDocument() throws SAXException {
        Enumeration e = tags.keys();
        while (e.hasMoreElements()) {
            String tag = (String)e.nextElement();
            int count = ((Integer)tags.get(tag)).intValue();
            System.out.println("Local Name \"" + tag + "\" occurs " 
                               + count + " times");
        }    
    }

    private static String convertToFileURL(String filename) {
        // ...
    }

    // ...
}

```

此代码定义了解析器遇到正在解析的文档的开始和结束点时应用程序执行的操作。`ContentHandler`接口的`startDocument()`方法创建了一个`java.util.Hashtable`实例，在元素事件中将填充解析器在文档中找到的 XML 元素。当解析器到达文档末尾时，将调用`endDocument()`方法，以获取哈希表中包含的元素的名称和计数，并在屏幕上打印一条消息，告诉用户找到每个元素的次数。

这两个`ContentHandler`方法都会抛出`SAXException`。您将在设置错误处理中了解更多关于 SAX 异常的信息。

### 元素事件

如文档事件中所述，`startDocument`方法创建的哈希表需要填充解析器在文档中找到的各种元素。以下代码处理了开始元素事件：

```java
public void startDocument() throws SAXException {
    tags = new Hashtable();
}

public void startElement(String namespaceURI,
                         String localName,
                         String qName, 
                         Attributes atts)
    throws SAXException {

    String key = localName;
    Object value = tags.get(key);

    if (value == null) {
        tags.put(key, new Integer(1));
    } 
    else {
        int count = ((Integer)value).intValue();
        count++;
        tags.put(key, new Integer(count));
    }
}

public void endDocument() throws SAXException {
    // ...
}

```

此代码处理元素标签，包括在开始标签中定义的任何属性，以获取该元素的命名空间统一资源标识符（URI）、本地名称和限定名称。然后，`startElement()`方法使用`startDocument()`创建的哈希映射填充每种类型元素的本地名称及其计数。请注意，当调用`startElement()`方法时，如果未启用命名空间处理，则元素和属性的本地名称可能为空字符串。代码通过在简单名称为空字符串时使用限定名称来处理该情况。

### 字符事件

JAXP SAX API 还允许您处理解析器传递给应用程序的字符，使用`ContentHandler.characters()`方法。

* * *

**注意 -** 字符事件在`SAXLocalNameCount`示例中没有展示，但为了完整起见，本节中包含了一个简要描述。

* * *

解析器不需要一次返回任何特定数量的字符。解析器可以一次返回一个字符，直到几千个字符，仍然是符合标准的实现。因此，如果您的应用程序需要处理它看到的字符，最好让`characters()`方法在`java.lang.StringBuffer`中累积字符，并且只在确定找到所有字符时对其进行操作。

当元素结束时，您完成了解析文本，因此通常在那时执行字符处理。但您可能还想在元素开始时处理文本。这对于包含与文本混合的 XML 元素的文档式数据是必要的。例如，考虑这个文档片段：

`**<para>**这段文字包含**<bold>**重要**</bold>**的想法。**</para>**`

初始文本`This paragraph contains`在`<bold>`元素的开始处终止。文本`important`在结束标签`</bold>`处终止，最终文本`ideas.`在结束标签`</para>`处终止。

严格来说，字符处理程序应该扫描`&`和`<`字符，并用适当的字符串`&amp;`或`&lt;`替换它们。这将在下一节中解释。

### 处理特殊字符

在 XML 中，实体是具有名称的 XML 结构（或纯文本）。通过名称引用实体会导致它在文档中插入实体引用的位置。要创建实体引用，您用和分号括起实体名称：

`&entityName;`

当您处理包含许多特殊字符的大块 XML 或 HTML 时，可以使用 CDATA 部分。CDATA 部分类似于 HTML 中的`<code>...</code>`，只是更加强大：CDATA 部分中的所有空格都是有意义的，并且其中的字符不会被解释为 XML。CDATA 郅始于`<![[CDATA[`，结束于`]]>`。

下面显示了一个 CDATA 部分的示例。

`<p><termdef id="dt-cdsection" term="CDATA Section">CDATA 部分</term> 可以出现在任何字符数据可能出现的地方；它们用于转义包含否则会被识别为标记的文本块。CDATA 郅始于字符串 "<code>&lt;![CDATA[</code>"，结束于字符串 "<code>]]&gt;</code>"`

解析后，此文本将显示如下：

CDATA 部分可以出现在任何字符数据可能出现的地方；它们用于转义包含否则会被识别为标记的文本块。CDATA 郅始于字符串 "`<![CDATA[`"，结束于字符串 "`]]>`"。

CDATA 的存在使得正确回显 XML 有点棘手。如果要输出的文本不在 CDATA 部分中，那么文本中的任何尖括号、和其他特殊字符应该被替换为适当的实体引用。（替换左尖括号和和符号是最重要的，其他字符将被正确解释而不会误导解析器。）但如果输出文本在 CDATA 部分中，那么不应进行替换，导致文本如前面的示例中所示。在像我们的 `SAXLocalNameCount` 应用程序这样的简单程序中，这并不特别严重。但许多 XML 过滤应用程序将希望跟踪文本是否出现在 CDATA 部分中，以便能够正确处理特殊字符。

## 设置解析器

以下代码设置解析器并启动它：

```java
static public void main(String[] args) throws Exception {

    // Code to parse command-line arguments 
    //(shown above)
    // ...

    SAXParserFactory spf = SAXParserFactory.newInstance();
    spf.setNamespaceAware(true);
    SAXParser saxParser = spf.newSAXParser();
}

```

这些代码行创建了一个 `SAXParserFactory` 实例，由 `javax.xml.parsers.SAXParserFactory` 系统属性的设置确定。通过将 `setNamespaceAware` 设置为 true 来设置要创建的工厂以支持 XML 命名空间，然后通过调用其 `newSAXParser()` 方法从工厂获取一个 `SAXParser` 实例。

* * *

**注意 -** `javax.xml.parsers.SAXParser` 类是一个包装器，定义了一些便利方法。它包装了（有点不太友好的）`org.xml.sax.Parser` 对象。如果需要，可以使用 `SAXParser` 类的 `getParser()` 方法获取该解析器。

* * *

现在您需要实现所有解析器必须实现的 `XMLReader`。`XMLReader` 由应用程序用于告诉 SAX 解析器对所讨论的文档执行什么处理。`XMLReader` 在 `main` 方法中通过以下代码实现。

```java
// ...
SAXParser saxParser = spf.newSAXParser();
XMLReader xmlReader = saxParser.getXMLReader();
xmlReader.setContentHandler(new SAXLocalNameCount());
xmlReader.parse(convertToFileURL(filename));

```

在这里，通过调用`SAXParser`实例的`getXMLReader()`方法获取用于解析器的`XMLReader`实例。然后，`XMLReader`将`SAXLocalNameCount`类注册为其内容处理程序，以便解析器执行的操作是处理内容事件中显示的`startDocument()`、`startElement()`和`endDocument()`方法。最后，`XMLReader`通过将 XML 文件的位置传递给解析器告诉解析器要解析哪个文档，传递的形式是由设置 I/O 中定义的`convertToFileURL()`方法生成的`File` URL。

## 设置错误处理

现在你可以开始使用你的解析器了，但最好实现一些错误处理。解析器可以生成三种错误：致命错误、错误和警告。当发生致命错误时，解析器无法继续。因此，如果应用程序没有生成异常，那么默认的错误事件处理程序会生成一个异常。但对于非致命错误和警告，默认错误处理程序永远不会生成异常，也不会显示任何消息。

如文档事件中所示，应用程序的事件处理方法会抛出`SAXException`。例如，`ContentHandler`接口中`startDocument()`方法的签名被定义为返回一个`SAXException`。

```java
public void startDocument() throws SAXException { /* ... */ }

```

可以使用消息、另一个异常或两者构造`SAXException`。

因为默认解析器仅为致命错误生成异常，并且默认解析器提供的有关错误的信息有些有限，`SAXLocalNameCount`程序通过`MyErrorHandler`类定义了自己的错误处理。

```java
xmlReader.setErrorHandler(new MyErrorHandler(System.err));

// ...

private static class MyErrorHandler implements ErrorHandler {
    private PrintStream out;

    MyErrorHandler(PrintStream out) {
        this.out = out;
    }

    private String getParseExceptionInfo(SAXParseException spe) {
        String systemId = spe.getSystemId();

        if (systemId == null) {
            systemId = "null";
        }

        String info = "URI=" + systemId + " Line=" 
            + spe.getLineNumber() + ": " + spe.getMessage();

        return info;
    }

    public void warning(SAXParseException spe) throws SAXException {
        out.println("Warning: " + getParseExceptionInfo(spe));
    }

    public void error(SAXParseException spe) throws SAXException {
        String message = "Error: " + getParseExceptionInfo(spe);
        throw new SAXException(message);
    }

    public void fatalError(SAXParseException spe) throws SAXException {
        String message = "Fatal Error: " + getParseExceptionInfo(spe);
        throw new SAXException(message);
    }
}

```

与设置解析器中展示`XMLReader`指向正确内容处理程序的方式相同，在这里通过调用其`setErrorHandler()`方法将`XMLReader`指向新的错误处理程序。

`MyErrorHandler`类实现了标准的`org.xml.sax.ErrorHandler`接口，并定义了一个方法来获取解析器生成的任何`SAXParseException`实例提供的异常信息。这个方法，`getParseExceptionInfo()`，简单地通过调用标准的`SAXParseException`方法`getLineNumber()`和`getSystemId()`获取错误发生在 XML 文档中的行号和运行它的系统的标识符。然后，这个异常信息被馈送到基本 SAX 错误处理方法`error()`、`warning()`和`fatalError()`的实现中，这些方法被更新以发送关于文档中错误的性质和位置的适当消息。

### 处理非致命错误

当 XML 文档未通过有效性约束时会发生非致命错误。如果解析器发现文档无效，则会生成一个错误事件。这样的错误是由验证解析器生成的，给定文档类型定义（DTD）或模式，当文档具有无效标记时，当找到不允许的标记时，或者（在模式的情况下）当元素包含无效数据时。

关于非致命错误最重要的原则是，默认情况下它们会被忽略。但如果文档中发生验证错误，你可能不希望继续处理它。你可能希望将这类错误视为致命错误。

要接管错误处理，你需要重写处理致命错误、非致命错误和警告的`DefaultHandler`方法，作为`ErrorHandler`接口的一部分。正如在前一节的代码片段中所示，SAX 解析器将`SAXParseException`传递给这些方法中的每一个，因此在发生错误时生成异常就像简单地将其抛回一样。

* * *

**注意 -** 检查`org.xml.sax.helpers.DefaultHandler`中定义的错误处理方法可能会很有启发性。你会发现`error()`和`warning()`方法什么都不做，而`fatalError()`会抛出异常。当然，你总是可以重写`fatalError()`方法以抛出不同的异常。但如果你的代码在发生致命错误时不抛出异常，那么 SAX 解析器会抛出异常。XML 规范要求如此。

* * *

### 处理警告

警告也会被默认忽略。警告是信息性的，只有在存在 DTD 或模式的情况下才能生成。例如，如果在 DTD 中两次定义了一个元素，则会生成警告。这并不是非法的，也不会引起问题，但你可能想知道，因为这可能不是有意的。将 XML 文档与 DTD 进行验证将在下一节中展示。

## 在没有验证的情况下运行 SAX 解析器示例

以下步骤解释了如何在没有验证的情况下运行 SAX 解析器示例。

### 在没有验证的情况下运行`SAXLocalNameCount`示例

1.  将`SAXLocalNameCount.java`文件保存在名为`sax`的目录中。

1.  编译文件如下：

    ```java
    javac sax/SAXLocalNameCount.java

    ```

1.  将示例 XML 文件`rich_iii.xml`和`two_gent.xml`保存在`data`目录中。

1.  在 XML 文件上运行`SAXLocalNameCount`程序。

    选择`data`目录中的一个 XML 文件，并在其上运行`SAXLocalNameCount`程序。在这里，我们选择在文件`rich_iii.xml`上运行该程序。

    ```java
    java sax/SAXLocalNameCount data/rich_iii.xml

    ```

    XML 文件`rich_iii.xml`包含了威廉·莎士比亚的剧作*理查三世*的 XML 版本。当你在其上运行`SAXLocalNameCount`时，你应该会看到以下输出。

    ```java
    Local Name "STAGEDIR" occurs 230 times
    Local Name "PERSONA" occurs 39 times
    Local Name "SPEECH" occurs 1089 times
    Local Name "SCENE" occurs 25 times
    Local Name "ACT" occurs 5 times
    Local Name "PGROUP" occurs 4 times
    Local Name "PLAY" occurs 1 times
    Local Name "PLAYSUBT" occurs 1 times
    Local Name "FM" occurs 1 times
    Local Name "SPEAKER" occurs 1091 times
    Local Name "TITLE" occurs 32 times
    Local Name "GRPDESCR" occurs 4 times
    Local Name "P" occurs 4 times
    Local Name "SCNDESCR" occurs 1 times
    Local Name "PERSONAE" occurs 1 times
    Local Name "LINE" occurs 3696 times

    ```

    `SAXLocalNameCount`程序解析 XML 文件，并提供每种类型的 XML 标记实例数量的计数。

1.  **在文本编辑器中打开文件`data/rich_iii.xml`。**

    为了检查错误处理是否有效，请从 XML 文件中删除一个条目的闭合标签，例如第 21 行显示的闭合标签`</PERSONA>`。

    `21 <PERSONA>爱德华，威尔士亲王，后来的国王爱德华五世。</PERSONA>`

1.  运行`SAXLocalNameCount`。

    这次，你应该看到以下致命错误消息。

    ```java
    Exception in thread "main" org.xml.sax.SAXException: Fatal Error: URI=file:data/rich_iii.xml Line=21: The element type "PERSONA" must be terminated by the matching end-tag "</PERSONA>".

    ```

    正如你所看到的，当遇到错误时，解析器生成了一个`SAXParseException`，这是`SAXException`的一个子类，用于标识错误发生的文件和位置。
