# 将 XML 数据读入 DOM

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/dom/readingXML.html`](https://docs.oracle.com/javase/tutorial/jaxp/dom/readingXML.html)

在本节中，您将通过读取现有的 XML 文件构造一个文档对象模型。

* * *

**注意 -** 在可扩展样式表语言转换中，您将看到如何将 DOM 写出为 XML 文件。（您还将看到如何相对容易地将现有数据文件转换为 XML。）

* * *

## 创建程序

文档对象模型提供了让您创建、修改、删除和重新排列节点的 API。在尝试创建 DOM 之前，了解 DOM 的结构是很有帮助的。这一系列示例将通过一个名为`DOMEcho`的示例程序展示 DOM 的内部结构，您可以在安装了 JAXP API 后在目录`*INSTALL_DIR*/jaxp-*version*/samples/dom`中找到它。

### 创建骨架

首先，构建一个简单的程序，将 XML 文档读入 DOM，然后再将其写回。

从应用程序的正常基本逻辑开始，并检查确保命令行上已提供了参数：

```java
public class DOMEcho {

    static final String outputEncoding = "UTF-8";

    private static void usage() {
        // ...
    }

    public static void main(String[] args) throws Exception {
        String filename = null;

        for (int i = 0; i < args.length; i++) {
            if (...) { 
                // ...
            } 
            else {
                filename = args[i];
                if (i != args.length - 1) {
                    usage();
                }
            }
        }

        if (filename == null) {
            usage();
        }
    }
}

```

此代码执行所有基本的设置操作。`DOMEcho`的所有输出都使用 UTF-8 编码。如果未指定参数，则调用`usage()`方法会简单地告诉您`DOMEcho`期望的参数，因此此处不显示代码。还声明了一个`filename`字符串，它将是要由`DOMEcho`解析为 DOM 的 XML 文件的名称。

### 导入所需的类

在本节中，所有类都以单独命名，以便您可以看到每个类来自何处，以便在需要引用 API 文档时参考。在示例文件中，导入语句使用较短的形式，如`javax.xml.parsers.*`。

这些是`DOMEcho`使用的 JAXP API：

```java
package dom;
import javax.xml.parsers.DocumentBuilder; 
import javax.xml.parsers.DocumentBuilderFactory;

```

这些类用于在解析 XML 文档时可能抛出的异常：

```java
import org.xml.sax.ErrorHandler;
import org.xml.sax.SAXException; 
import org.xml.sax.SAXParseException;
import org.xml.sax.helpers.*

```

这些类读取示例 XML 文件并管理输出：

```java
import java.io.File;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;

```

最后，导入 W3C 定义的 DOM、DOM 异常、实体和节点：

```java
import org.w3c.dom.Document;
import org.w3c.dom.DocumentType;
import org.w3c.dom.Entity;
import org.w3c.dom.NamedNodeMap;
import org.w3c.dom.Node;

```

### 处理错误

接下来，添加错误处理逻辑。最重要的一点是，当 JAXP 符合标准的文档构建器在解析 XML 文档时遇到问题时，需要报告 SAX 异常。DOM 解析器实际上不必在内部使用 SAX 解析器，但由于 SAX 标准已经存在，因此使用它来报告错误是有意义的。因此，DOM 应用程序的错误处理代码与 SAX 应用程序的错误处理代码非常相似：

```java
private static class MyErrorHandler implements ErrorHandler {

    private PrintWriter out;

    MyErrorHandler(PrintWriter out) {
        this.out = out;
    }

    private String getParseExceptionInfo(SAXParseException spe) {
        String systemId = spe.getSystemId();
        if (systemId == null) {
            systemId = "null";
        }

        String info = "URI=" + systemId + " Line=" + spe.getLineNumber() +
                      ": " + spe.getMessage();
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

正如您所看到的，`DomEcho`类的错误处理程序使用`PrintWriter`实例生成其输出。

### 实例化工厂

接下来，在`main()`方法中添加以下代码，以获取一个可以提供文档构建器的工厂实例。

```java
public static void main(String[] args) throws Exception {
    DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

    // ...
}

```

### 获取解析器并解析文件

现在，在`main()`中添加以下代码以获取一个构建器实例，并使用它来解析指定的文件。

```java
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
DocumentBuilder db = dbf.newDocumentBuilder(); 
Document doc = db.parse(new File(filename));

```

被解析的文件由在 `main()` 方法开头声明的 `filename` 变量提供，当程序运行时，它作为参数传递给 `DOMEcho`。

### 配置工厂

默认情况下，工厂返回一个不进行验证的解析器，不了解命名空间。要获得一个验证解析器，或者一个了解命名空间的解析器（或两者兼有），您可以配置工厂来设置这两个选项中的一个或两个，使用以下代码。

```java
public static void main(String[] args) throws Exception {

    String filename = null;
    boolean dtdValidate = false;
    boolean xsdValidate = false;
    String schemaSource = null;

    for (int i = 0; i < args.length; i++) {
        if (args[i].equals("-dtd"))  { 
            dtdValidate = true;
        } 
        else if (args[i].equals("-xsd")) {
            xsdValidate = true;
        } 
        else if (args[i].equals("-xsdss")) {
            if (i == args.length - 1) {
                usage();
            }
            xsdValidate = true;
            schemaSource = args[++i];
        }
        else {
            filename = args[i];
            if (i != args.length - 1) {
                usage();
            }
        }
    }

    if (filename == null) {
        usage();
    }

    DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

    dbf.setNamespaceAware(true);
    dbf.setValidating(dtdValidate || xsdValidate);

    // ...

    DocumentBuilder db = dbf.newDocumentBuilder();
    Document doc = db.parse(new File(filename));
}

```

如您所见，命令行参数已设置好，以便您可以通知 `DOMEcho` 对 DTD 或 XML Schema 执行验证，并且工厂已配置为了解命名空间并执行用户指定的验证类型。

* * *

**注意 -** 符合 JAXP 标准的解析器并不需要支持所有这些选项的所有组合，即使参考解析器支持。如果您指定了无效的选项组合，在尝试获取解析器实例时，工厂会生成一个 `ParserConfigurationException`。

* * *

有关如何使用命名空间和验证的更多信息，请参阅使用 XML Schema 进行验证，其中将描述上述摘录中缺失的代码。

### 处理验证错误

根据 SAX 标准规定，对验证错误的默认响应是不执行任何操作。JAXP 标准要求抛出 SAX 异常，因此您使用与 SAX 应用程序相同的错误处理机制。特别是，您使用 `DocumentBuilder` 类的 `setErrorHandler` 方法来提供一个实现 SAX `ErrorHandler` 接口的对象。

* * *

**注意 -** `DocumentBuilder` 还有一个 `setEntityResolver` 方法可供使用。

* * *

以下代码配置文档构建器使用在处理错误中定义的错误处理程序。

```java
DocumentBuilder db = dbf.newDocumentBuilder();
OutputStreamWriter errorWriter = new OutputStreamWriter(System.err,
                                         outputEncoding);
db.setErrorHandler(new MyErrorHandler (new PrintWriter(errorWriter, true)));
Document doc = db.parse(new File(filename));

```

到目前为止，您看到的代码已经设置了文档构建器，并配置它在请求时执行验证。错误处理也已就位。然而，`DOMEcho` 还没有做任何事情。在下一节中，您将看到如何显示 DOM 结构并开始探索它。例如，您将看到在 DOM 中实体引用和 CDATA 部分的样子。也许最重要的是，您将看到文本节点（包含实际数据）如何存在于 DOM 中的元素节点下。

## 显示 DOM 节点

要创建或操作 DOM，有一个清晰的关于 DOM 中节点结构的概念是很有帮助的。本教程的这一部分揭示了 DOM 的内部结构，这样你就可以看到它包含的内容。`DOMEcho` 示例通过回显 DOM 节点，然后在屏幕上打印出来，适当缩进以使节点层次结构明显可见。这些节点类型的规范可以在[DOM Level 2 Core Specification](http://www.w3.org/TR/2000/REC-DOM-Level-2-Core-20001113)中找到，在`Node`规范下。下面的表 3-1 是从该规范中调整过来的。

表 3-1 节点类型

| Node | 节点名称 | 节点值 | 属性 |
| --- | --- | --- | --- |
| `Attr` | 属性名称 | 属性值 | null |
| `CDATASection` | `#cdata-section` | CDATA 部分的内容 | null |
| `Comment` | `#comment` | 注释的内容 | null |
| `Document` | `#document` | null | null |
| `DocumentFragment` | `#documentFragment` | null | null |
| `DocumentType` | 文档类型名称 | null | null |
| `Element` | 标签名称 | null | null |
| `Entity` | 实体名称 | null | null |
| `EntityReference` | 引用的实体名称 | null | null |
| `Notation` | 符号名称 | null | null |
| `ProcessingInstruction` | 目标 | 不包括目标的整个内容 | null |
| `Text` | `#text` | 文本节点的内容 | null |

此表中的信息非常有用；在处理 DOM 时，你将需要它，因为所有这些类型都混合在 DOM 树中。

### 获取节点类型信息

通过调用`org.w3c.dom.Node`类的各种方法来获取 DOM 节点元素类型信息。`DOMEcho`暴露的节点属性由以下代码回显。

```java
private void printlnCommon(Node n) {
    out.print(" nodeName=\"" + n.getNodeName() + "\"");

    String val = n.getNamespaceURI();
    if (val != null) {
        out.print(" uri=\"" + val + "\"");
    }

    val = n.getPrefix();

    if (val != null) {
        out.print(" pre=\"" + val + "\"");
    }

    val = n.getLocalName();
    if (val != null) {
        out.print(" local=\"" + val + "\"");
    }

    val = n.getNodeValue();
    if (val != null) {
        out.print(" nodeValue=");
        if (val.trim().equals("")) {
            // Whitespace
            out.print("[WS]");
        }
        else {
            out.print("\"" + n.getNodeValue() + "\"");
        }
    }
    out.println();
}

```

每个 DOM 节点至少有一个类型、一个名称和一个值，这个值可能为空也可能不为空。在上面的示例中，`Node`接口的`getNamespaceURI()`、`getPrefix()`、`getLocalName()`和`getNodeValue()`方法返回并打印回显节点的命名空间 URI、命名空间前缀、本地限定名称和值。请注意，对`getNodeValue()`返回的值调用`trim()`方法，以确定节点的值是否为空白字符，并相应地打印消息。

要查看`Node`方法的完整列表以及它们返回的不同信息，请参阅[`Node`](https://docs.oracle.com/javase/8/docs/api/org/w3c/dom/Node.html)的 API 文档。

接下来，定义一个方法来设置节点打印时的缩进，以便节点层次结构能够清晰可见。

```java
private void outputIndentation() {
    for (int i = 0; i < indent; i++) {
        out.print(basicIndent);
    }
}

```

当`DOMEcho`显示节点树层次结构时，使用的基本缩进单位由`DOMEcho`构造函数类中添加以下突出显示的行来定义`basicIndent`常量。

```java
public class DOMEcho {
    static final String outputEncoding = "UTF-8";

    private PrintWriter out;
    private int indent = 0;
    private final String basicIndent = " ";

    DOMEcho(PrintWriter out) {
        this.out = out;
    }
}

```

就像在处理错误中定义的错误处理程序一样，`DOMEcho` 程序将创建其输出作为 `PrintWriter` 实例。

### 词法控制

词法信息是您需要重建 XML 文档原始语法的信息。在编辑应用程序中保留词法信息非常重要，因为您希望保存的文档是对原始文档的准确反映-包括注释、实体引用以及一开始可能包含的任何 CDATA 部分。

然而，大多数应用程序只关注 XML 结构的内容。它们可以忽略注释，并且不在乎数据是在 CDATA 部分中编码还是作为纯文本，或者是否包含实体引用。对于这类应用程序，最好保留最少的词法信息，因为这简化了应用程序必须准备检查的 DOM 节点的数量和类型。

以下`DocumentBuilderFactory`方法让您控制在 DOM 中看到的词法信息。

`setCoalescing()`

将`CDATA`节点转换为`Text`节点并附加到相邻的`Text`节点（如果有）。

`setExpandEntityReferences()`

为了扩展实体引用节点。

`setIgnoringComments()`

忽略注释。

`setIgnoringElementContentWhitespace()`

忽略不是元素内容的空白。

所有这些属性的默认值都是 false，这保留了重建传入文档所需的所有词法信息，以其原始形式。将它们设置为 true 可以构建最简单的 DOM，以便应用程序可以专注于数据的语义内容，而不必担心词法语法细节。表 3-2 总结了设置的效果。

表 3-2 词法控制设置

| API | 保留词法信息 | 关注内容 |
| --- | --- | --- |
| `setCoalescing()` | False | True |
| `setExpandEntityReferences()` | False | True |
| `setIgnoringComments()` | False | True |
| `setIgnoringElementContent``Whitespace()` | False | True |

这些方法在`DomEcho`示例的主方法中的实现如下所示。

```java
// ...

dbf.setIgnoringComments(ignoreComments);
dbf.setIgnoringElementContentWhitespace(ignoreWhitespace);
dbf.setCoalescing(putCDATAIntoText);
dbf.setExpandEntityReferences(!createEntityRefs);

// ...

```

布尔变量`ignoreComments`、`ignoreWhitespace`、`putCDATAIntoText`和`createEntityRefs`在主方法代码的开头声明，并且当运行`DomEcho`时通过命令行参数设置。

```java
public static void main(String[] args) throws Exception {
    // ...

    boolean ignoreWhitespace = false;
    boolean ignoreComments = false;
    boolean putCDATAIntoText = false;
    boolean createEntityRefs = false;

    for (int i = 0; i < args.length; i++) {
        if (...) {  // Validation arguments here
           // ... 
        } 
        else if (args[i].equals("-ws")) {
            ignoreWhitespace = true;
        } 
        else if (args[i].startsWith("-co")) {
            ignoreComments = true;
        }
        else if (args[i].startsWith("-cd")) {
            putCDATAIntoText = true;
        } 
        else if (args[i].startsWith("-e")) {
            createEntityRefs = true;

            // ...
        } 
        else {
            filename = args[i];

            // Must be last arg
            if (i != args.length - 1) {
                usage();
            }
        }
    }

    // ...
}

```

## 打印 DOM 树节点

`DomEcho`应用程序允许您查看 DOM 的结构，并演示了 DOM 由哪些节点组成以及它们是如何排列的。一般来说，DOM 树中绝大多数节点将是`Element`和`Text`节点。

* * *

**注意 -** 文本节点存在于 DOM 中的元素节点**下方**，数据始终存储在文本节点中。在 DOM 处理中最常见的错误可能是导航到元素节点并期望它包含存储在该元素中的数据。事实并非如此！即使是最简单的元素节点下面也有一个包含数据的文本节点。

* * *

打印 DOM 树节点的代码以适当的缩进显示如下。

```java
private void echo(Node n) {
    outputIndentation();
    int type = n.getNodeType();

    switch (type) {
        case Node.ATTRIBUTE_NODE:
            out.print("ATTR:");
            printlnCommon(n);
            break;

        case Node.CDATA_SECTION_NODE:
            out.print("CDATA:");
            printlnCommon(n);
            break;

        case Node.COMMENT_NODE:
            out.print("COMM:");
            printlnCommon(n);
            break;

        case Node.DOCUMENT_FRAGMENT_NODE:
            out.print("DOC_FRAG:");
            printlnCommon(n);
            break;

        case Node.DOCUMENT_NODE:
            out.print("DOC:");
            printlnCommon(n);
            break;

        case Node.DOCUMENT_TYPE_NODE:
            out.print("DOC_TYPE:");
            printlnCommon(n);
            NamedNodeMap nodeMap = ((DocumentType)n).getEntities();
            indent += 2;
            for (int i = 0; i < nodeMap.getLength(); i++) {
                Entity entity = (Entity)nodeMap.item(i);
                echo(entity);
            }
            indent -= 2;
            break;

        case Node.ELEMENT_NODE:
            out.print("ELEM:");
            printlnCommon(n);

            NamedNodeMap atts = n.getAttributes();
            indent += 2;
            for (int i = 0; i < atts.getLength(); i++) {
                Node att = atts.item(i);
                echo(att);
            }
            indent -= 2;
            break;

        case Node.ENTITY_NODE:
            out.print("ENT:");
            printlnCommon(n);
            break;

        case Node.ENTITY_REFERENCE_NODE:
            out.print("ENT_REF:");
            printlnCommon(n);
            break;

        case Node.NOTATION_NODE:
            out.print("NOTATION:");
            printlnCommon(n);
            break;

        case Node.PROCESSING_INSTRUCTION_NODE:
            out.print("PROC_INST:");
            printlnCommon(n);
            break;

        case Node.TEXT_NODE:
            out.print("TEXT:");
            printlnCommon(n);
            break;

        default:
            out.print("UNSUPPORTED NODE: " + type);
            printlnCommon(n);
            break;
    }

    indent++;
    for (Node child = n.getFirstChild(); child != null;
         child = child.getNextSibling()) {
        echo(child);
    }
    indent--;
}

```

该代码首先使用 switch 语句打印出不同的节点类型和任何可能的子节点，并进行适当的缩进。

节点属性不包括在 DOM 层次结构的子节点中。而是通过`Node`接口的`getAttributes`方法获取。

`DocType`接口是`w3c.org.dom.Node`的扩展。它定义了`getEntities`方法，您可以使用该方法获取`Entity`节点 - 定义实体的节点。与`Attribute`节点一样，`Entity`节点不会出现为 DOM 节点的子节点。

## 节点操作

本节简要介绍了您可能想要应用于 DOM 的一些操作。

+   创建节点

+   遍历节点

+   搜索节点

+   获取节点内容

+   创建属性

+   删除和更改节点

+   插入节点

### 创建节点

您可以使用`Document`接口的方法创建不同类型的节点。例如，`createElement`、`createComment`、`createCDATAsection`、`createTextNode`等。有关创建不同节点的方法的完整列表，请参阅[`org.w3c.dom.Document`](https://docs.oracle.com/javase/8/docs/api/org/w3c/dom/Document.html)的 API 文档。

### 遍历节点

`org.w3c.dom.Node`接口定义了一些方法，您可以使用这些方法遍历节点，包括`getFirstChild`、`getLastChild`、`getNextSibling`、`getPreviousSibling`和`getParentNode`。这些操作足以从树中的任何位置到达树中的任何其他位置。

### 搜索节点

当您搜索具有特定名称的节点时，需要考虑更多因素。虽然诱人的做法是获取第一个子节点并检查它是否正确，但搜索必须考虑到子列表中的第一个子节点可能是注释或处理指令。如果 XML 数据尚未经过验证，甚至可能是包含可忽略空格的文本节点。

本质上，您需要查看子节点列表，忽略那些不相关的节点，并检查您关心的节点。以下是在 DOM 层次结构中搜索节点时需要编写的一种例程。它在这里完整呈现（包括注释），以便您可以将其用作应用程序中的模板。

```java
/**
 * Find the named subnode in a node's sublist.
 * <ul>
 * <li>Ignores comments and processing instructions.
 * <li>Ignores TEXT nodes (likely to exist and contain
 *         ignorable whitespace, if not validating.
 * <li>Ignores CDATA nodes and EntityRef nodes.
 * <li>Examines element nodes to find one with
 *        the specified name.
 * </ul>
 * @param name  the tag name for the element to find
 * @param node  the element node to start searching from
 * @return the Node found
 */
public Node findSubNode(String name, Node node) {
    if (node.getNodeType() != Node.ELEMENT_NODE) {
        System.err.println("Error: Search node not of element type");
        System.exit(22);
    }

    if (! node.hasChildNodes()) return null;

    NodeList list = node.getChildNodes();
    for (int i=0; i < list.getLength(); i++) {
        Node subnode = list.item(i);
        if (subnode.getNodeType() == Node.ELEMENT_NODE) {
           if (subnode.getNodeName().equals(name)) 
               return subnode;
        }
    }
    return null;
}

```

要深入了解此代码，请参阅增加复杂性中的何时使用 DOM。此外，您还可以使用词法控制中描述的 API 来修改解析器构造的 DOM 类型。不过，这段代码的好处是几乎适用于任何 DOM。

### 获取节点内容

当您想要获取节点包含的文本时，您需要再次查看子节点列表，忽略不相关的条目，并在`TEXT`节点、`CDATA`节点和`EntityRef`节点中找到的文本累积起来。以下是您可以用于该过程的一种例程。

```java
/**
  * Return the text that a node contains. This routine:
  * <ul>
  * <li>Ignores comments and processing instructions.
  * <li>Concatenates TEXT nodes, CDATA nodes, and the results of
  *     recursively processing EntityRef nodes.
  * <li>Ignores any element nodes in the sublist.
  *     (Other possible options are to recurse into element 
  *      sublists or throw an exception.)
  * </ul>
  * @param    node  a  DOM node
  * @return   a String representing its contents
  */
public String getText(Node node) {
    StringBuffer result = new StringBuffer();
    if (! node.hasChildNodes()) return "";

    NodeList list = node.getChildNodes();
    for (int i=0; i < list.getLength(); i++) {
        Node subnode = list.item(i);
        if (subnode.getNodeType() == Node.TEXT_NODE) {
            result.append(subnode.getNodeValue());
        }
        else if (subnode.getNodeType() == Node.CDATA_SECTION_NODE) {
            result.append(subnode.getNodeValue());
        }
        else if (subnode.getNodeType() == Node.ENTITY_REFERENCE_NODE) {
            // Recurse into the subtree for text
            // (and ignore comments)
            result.append(getText(subnode));
        }
    }

    return result.toString();
}

```

关于这段代码的更深入解释，请参见增加复杂性中的何时使用 DOM。同样，你可以通过使用词法控制中描述的 API 来简化这段代码，以修改解析器构造的 DOM 类型。但这段代码的好处是几乎适用于任何 DOM。

### 创建属性

扩展了 Node 接口的`org.w3c.dom.Element`接口定义了一个`setAttribute`操作，用于向该节点添加属性。（从 Java 平台的角度来看，更好的名称应该是`addAttribute`。该属性不是类的属性，而是创建了一个新对象。）你还可以使用`Document`的`createAttribute`操作来创建`Attribute`的实例，然后使用`setAttributeNode`方法来添加它。

### 删除和更改节点

要删除一个节点，你可以使用其父节点的`removeChild`方法。要更改它，你可以使用父节点的`replaceChild`操作或节点的`setNodeValue`操作。

### 插入节点

在创建新节点时要记住的重要事情是，当你创建一个元素节点时，你只需指定一个名称。实际上，该节点给你提供了一个挂载物件的钩子。你可以通过向其子节点列表添加内容来将物件挂在钩子上。例如，你可以添加一个文本节点、一个`CDATA`节点或一个属性节点。在构建过程中，请记住你在本教程中看到的结构。记住：层次结构中的每个节点都非常简单，只包含一个数据元素。

## 运行`DOMEcho`示例

要运行`DOMEcho`示例，请按照以下步骤操作。

1.  **导航至`samples`目录。**`% cd *install-dir*`/jaxp-1_4_2-`*release-date*`/samples`.`

1.  **编译示例类。**`% javac dom/*`

1.  **在 XML 文件上运行`DOMEcho`程序。**

    选择`data`目录中的一个 XML 文件，并在其上运行`DOMEcho`程序。在这里，我们选择在文件`personal-schema.xml`上运行该程序。

    `% java dom/DOMEcho data/personal-schema.xml`

    XML 文件`personal-schema.xml`包含了一个小公司的人员档案。当你在其上运行`DOMEcho`程序时，你应该看到以下输出。

    ```java
    DOC: nodeName="#document"
     ELEM: nodeName="personnel" 
           local="personnel"
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     ELEM: nodeName="person" 
           local="person"
     ATTR: nodeName="id" 
           local="id" 
           nodeValue="Big.Boss"
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     ELEM: nodeName="name" 
           local="name"
     ELEM: nodeName="family" 
           local="family"
     TEXT: nodeName="#text" 
           nodeValue="Boss"
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     ELEM: nodeName="given" 
           local="given"
     TEXT: nodeName="#text" 
           nodeValue="Big"
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     ELEM: nodeName="email" 
           local="email"
     TEXT: nodeName="#text" 
           nodeValue="chief@foo.example.com"
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     ELEM: nodeName="link" 
           local="link"
     ATTR: nodeName="subordinates" 
           local="subordinates" 
           nodeValue="one.worker two.worker 
                      three.worker four.worker
                      five.worker"
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     ELEM: nodeName="person" 
           local="person"
     ATTR: nodeName="id" 
           local="id" 
           nodeValue="one.worker"
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     ELEM: nodeName="name" 
           local="name"
     ELEM: nodeName="family" 
           local="family"
     TEXT: nodeName="#text" 
           nodeValue="Worker"
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     ELEM: nodeName="given" 
           local="given"
     TEXT: nodeName="#text" 
           nodeValue="One"
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     ELEM: nodeName="email" 
           local="email"
     TEXT: nodeName="#text" 
           nodeValue="one@foo.example.com"
     TEXT: nodeName="#text" 
           nodeValue=[WS]
     ELEM: nodeName="link" 
           local="link"
     ATTR: nodeName="manager" 
           local="manager" 
           nodeValue="Big.Boss"
     TEXT: nodeName="#text"
           nodeValue=[WS]

    [...]

    ```

    正如你所看到的，`DOMEcho`打印出文档中不同元素的所有节点，并正确缩进以显示节点层次结构。
