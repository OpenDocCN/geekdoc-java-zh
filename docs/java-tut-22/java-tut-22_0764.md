# 实现 SAX 验证

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/sax/validation.html`](https://docs.oracle.com/javase/tutorial/jaxp/sax/validation.html)

示例程序`SAXLocalNameCount`默认使用非验证解析器，但也可以激活验证。激活验证允许应用程序判断 XML 文档是否包含正确的标记，或这些标记是否按正确顺序出现。换句话说，它可以告诉您文档是否*有效*。然而，如果未激活验证，它只能告诉文档是否格式良好，就像在上一节中删除 XML 元素的闭合标记时所示。要进行验证，XML 文档需要关联到一个 DTD 或 XML 模式。`SAXLocalNameCount`程序可以选择这两个选项。

## 选择解析器实现

如果没有指定其他工厂类，则将使用默认的`SAXParserFactory`类。要使用来自不同制造商的解析器，可以更改指向其的环境变量的值。您可以从命令行执行：

```java
java -Djavax.xml.parsers.SAXParserFactory=*yourFactoryHere* [...]

```

您指定的工厂名称必须是完全限定的类名（包括所有包前缀）。有关更多信息，请参阅`SAXParserFactory`类的`newInstance()`方法中的文档。

## 使用验证解析器

直到这一点，本课程一直集中在非验证解析器上。本节将检查验证解析器，以了解在使用它解析示例程序时会发生什么。

关于验证解析器必须理解的两件事：

+   需要模式或 DTD。

+   因为存在模式或 DTD，只要可能，`ContentHandler.``ignorableWhitespace()`方法就会被调用。

## 可忽略的空格

当存在 DTD 时，解析器将不再调用`characters()`方法处理它知道是无关紧要的空格。从只对 XML 数据感兴趣的应用程序的角度来看，这是一件好事，因为应用程序永远不会受到纯粹为了使 XML 文件可读而存在的空格的干扰。

另一方面，如果您正在编写一个过滤 XML 数据文件的应用程序，并且希望输出一个同样可读的文件版本，那么这些空格将不再是无关紧要的：它们将变得至关重要。要获取这些字符，您需要在应用程序中添加`ignorableWhitespace`方法。为了处理解析器看到的任何（通常是）可忽略的空格，您需要添加类似以下代码以实现`ignorableWhitespace`事件处理程序。

```java
public void ignorableWhitespace (char buf[], int start, int length) throws SAXException { 
    emit("IGNORABLE");
}

```

这段代码只是生成一条消息，让您知道看到了可忽略的空格。然而，并非所有解析器都是平等的。SAX 规范不要求调用此方法。Java XML 实现在 DTD 可能时会这样做。

## 配置工厂

`SAXParserFactory` 需要设置为使用验证解析器而不是默认的非验证解析器。 下面是 `SAXLocalNameCount` 示例的 `main()` 方法中的代码，显示了如何配置工厂以实现验证解析器。

```java
static public void main(String[] args) throws Exception {

    String filename = null;
    boolean dtdValidate = false;
    boolean xsdValidate = false;
    String schemaSource = null;

    for (int i = 0; i < args.length; i++) {

        if (args[i].equals("-dtd")) {
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
        else if (args[i].equals("-usage")) {
            usage();
        }
        else if (args[i].equals("-help")) {
            usage();
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

    SAXParserFactory spf = SAXParserFactory.newInstance();
    spf.setNamespaceAware(true);
    spf.setValidating(dtdValidate || xsdValidate);
    SAXParser saxParser = spf.newSAXParser();

    // ... 
}

```

在这里，`SAXLocalNameCount` 程序被配置为在启动时接受额外的参数，告诉它实现无验证、DTD 验证、XML Schema 定义（XSD）验证或针对特定模式源文件的 XSD 验证。 （这些选项的描述 `-dtd`、`-xsd` 和 `-xsdss` 也被添加到 `usage()` 方法中，但这里没有显示。）然后，工厂被配置为在调用 `newSAXParser` 时生成适当的验证解析器。 如 设置解析器 中所示，您还可以使用 `setNamespaceAware(true)` 来配置工厂返回一个支持命名空间的解析器。 Oracle 的实现支持任何配置选项的组合。 （如果某个实现不支持特定组合，则需要生成工厂配置错误）。

## 使用 XML Schema 进行验证

尽管本教程不涵盖 XML Schema 的完整内容，但本节向您展示了使用 XML Schema 语言编写的现有模式验证 XML 文档的步骤。 要了解有关 XML Schema 的更多信息，您可以查看在线教程 *XML Schema Part 0: Primer*，网址为 [`www.w3.org/TR/xmlschema-0/`](http://www.w3.org/TR/xmlschema-0/)。

* * *

**注意 -** 存在多种模式定义语言，包括 RELAX NG、Schematron 和 W3C 的 "XML Schema" 标准。 （即使 DTD 也算是一种 "模式"，尽管它是唯一不使用 XML 语法描述模式约束的模式。）然而，"XML Schema" 给我们带来了一个术语上的挑战。 虽然短语 "XML Schema schema" 可以很精确，但我们将使用短语 "XML Schema definition" 来避免冗余的外观。

* * *

要在 XML 文档中收到验证错误的通知，解析器工厂必须配置为创建验证解析器，如前一节所示。 此外，以下条件必须成立：

+   在 SAX 解析器上必须设置适当的属性。

+   必须设置适当的错误处理程序。

+   文档必须与模式关联。

## 设置 SAX 解析器属性

从定义将用于设置属性的常量开始是有帮助的。 `SAXLocalNameCount` 示例设置了以下常量。

```java
public class SAXLocalNameCount extends DefaultHandler {

    static final String JAXP_SCHEMA_LANGUAGE =
        "http://java.sun.com/xml/jaxp/properties/schemaLanguage";

    static final String W3C_XML_SCHEMA =
        "http://www.w3.org/2001/XMLSchema";

    static final String JAXP_SCHEMA_SOURCE =
        "http://java.sun.com/xml/jaxp/properties/schemaSource";
}

```

* * *

**注意 -** 解析器工厂必须配置为生成一个既具有命名空间感知能力又进行验证的解析器。这在配置工厂中已经展示过。有关命名空间的更多信息在文档对象模型中提供，但现在，请理解模式验证是一个面向命名空间的过程。因为符合 JAXP 规范的解析器默认情况下不具有命名空间感知能力，所以必须设置用于模式验证的属性才能正常工作。

* * *

然后，您必须配置解析器以告诉它要使用哪种模式语言。在`SAXLocalNameCount`中，验证可以针对 DTD 或 XML Schema 执行。以下代码使用上面定义的常量来指定 W3C 的 XML Schema 语言作为程序启动时指定`-xsd`选项时要使用的语言。

```java
// ...
if (xsdValidate) {
    saxParser.setProperty(JAXP_SCHEMA_LANGUAGE, W3C_XML_SCHEMA);
    // ...
}

```

除了设置错误处理中描述的错误处理外，在配置解析器进行基于模式的验证时可能会发生一个错误。如果解析器不符合 JAXP 规范，因此不支持 XML Schema，它可能会抛出`SAXNotRecognizedException`。为了处理这种情况，`setProperty()`语句被包裹在 try/catch 块中，如下面的代码所示。

```java
// ...
if (xsdValidate) {
    try {
        saxParser.setProperty(JAXP_SCHEMA_LANGUAGE, W3C_XML_SCHEMA);
    }
    catch (SAXNotRecognizedException x){
        System.err.println("Error: JAXP SAXParser property not recognized: "
                           + JAXP_SCHEMA_LANGUAGE);

        System.err.println( "Check to see if parser conforms to the JAXP spec.");
        System.exit(1);
    }
}
// ...

```

## 将文档与模式关联

要使用 XML Schema 定义验证数据，必须确保 XML 文档与之关联。有两种方法可以做到这一点。

+   通过在 XML 文档中包含模式声明。

+   通过在应用程序中指定要使用的模式。

* * *

**注意 -** 当应用程序指定要使用的模式时，它会覆盖文档中的任何模式声明。

* * *

要在文档中指定模式定义，您可以创建如下的 XML：

```java
<*documentRoot*

    xsi:noNamespaceSchemaLocation='*YourSchemaDefinition*.xsd'>

```

第一个属性定义了 XML 命名空间（`xmlns`）前缀`xsi`，代表 XML Schema 实例。第二行指定了要用于文档中没有命名空间前缀的元素的模式，即通常在任何简单、不复杂的 XML 文档中定义的元素。

* * *

**注意 -** 有关命名空间的更多信息包含在文档对象模型中的使用 XML Schema 进行验证中。现在，将这些属性视为您用来验证不使用它们的简单 XML 文件的"魔法咒语"。在学习更多关于命名空间的知识后，您将了解如何使用 XML Schema 来验证使用它们的复杂文档。这些想法在文档对象模型的使用多个命名空间进行验证中讨论。

* * *

您还可以在应用程序中指定模式文件，就像在`SAXLocalNameCount`中一样。

```java
// ...
if (schemaSource != null) {
    saxParser.setProperty(JAXP_SCHEMA_SOURCE, new File(schemaSource));
}

// ...

```

在上面的代码中，变量`schemaSource`与一个模式源文件相关联，您可以通过使用`-xsdss`选项启动`SAXLocalNameCount`应用程序并提供要使用的模式源文件的名称来指向它。

## 验证解析器中的错误处理

重要的是要认识到，当文件未通过验证时抛出异常的唯一原因是由于设置错误处理中显示的错误处理代码。该代码在此作为提醒再次显示：

```java
// ...

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

// ...

```

如果这些异常没有被抛出，则验证错误将被简单地忽略。一般来说，SAX 解析错误是验证错误，尽管如果文件指定了解析器无法处理的 XML 版本，也会生成它。请记住，除非提供类似这里的错误处理程序，否则您的应用程序不会生成验证异常。

## DTD 警告

如前所述，警告仅在 SAX 解析器处理 DTD 时生成。某些警告仅由验证解析器生成。非验证解析器的主要目标是尽可能快地运行，但它也会生成一些警告。

XML 规范建议由于以下原因生成警告：

+   为实体、属性或符号提供额外的声明。（这些声明将被忽略。仅使用第一个。此外，请注意，当验证时，元素的重复定义总是会产生致命错误，就像您之前看到的那样。）

+   引用未声明的元素类型。（仅当未声明的类型实际在 XML 文档中使用时才会发生有效性错误。当在 DTD 中引用未声明的元素时会产生警告。）

+   为未声明的元素类型声明属性。

Java XML SAX 解析器还在其他情况下发出警告：

+   在验证时不需要`<!DOCTYPE ...>`。

+   在不进行验证时引用未定义的参数实体。（在验证时会产生错误。尽管非验证解析器不需要读取参数实体，但 Java XML 解析器会这样做。因为这不是一个要求，所以 Java XML 解析器生成警告，而不是错误。）

+   某些情况下，字符编码声明看起来不正确。

## 运行带验证的 SAX 解析器示例

在本节中，之前使用的`SAXLocalNameCount`示例程序将再次被使用，但这次将针对 XML Schema 或 DTD 进行验证。展示不同类型的验证的最佳方式是修改被解析的 XML 文件的代码，以及相关的模式和 DTD，以破坏处理并使应用程序生成异常。

## 尝试使用 DTD 验证错误进行实验

如上所述，这些示例重用了`SAXLocalNameCount`程序。您将在不进行验证运行 SAX 解析器示例中找到示例及其相关文件的位置。

1.  如果尚未这样做，请将`SAXLocalNameCount.java`文件保存在名为`sax`的目录中。在文本编辑器中打开文件并进行上述更改。

1.  如果尚未这样做，请按以下方式编译文件：

    ```java
    javac sax/SAXLocalNameCount.java

    ```

1.  如果尚未这样做，请将示例 XML 文件``rich_iii.xml``和``two_gent.xml``保存在`data`目录中。

1.  运行带有 DTD 验证的`SAXLocalNameCount`程序。

    要做到这一点，运行程序时必须指定`-dtd`选项。

    ```java
    java sax/SAXLocalNameCount -dtd data/rich_iii.xml

    ```

    您看到的结果将类似于这样：

    ```java
    Exception in thread "main" org.xml.sax.SAXException:
    Error: URI=file:data/rich_iii.xml 
    Line=4: Document is invalid: no grammar found.

    ```

    此消息表示没有语法可以针对文档`rich_iii.xml`进行验证，因此它自动无效。换句话说，该消息表示您正在尝试验证文档，但没有声明 DTD，因为没有`DOCTYPE`声明。因此，现在您知道 DTD 是有效文档的要求。这是有道理的。

1.  将示例 DTD 文件``play.dtd``保存在`data`目录中。

1.  在文本编辑器中打开文件`data/rich_iii.xml`。在`data/rich_iii.xml`的开头插入以下`DOCTYPE`声明。（该声明将验证解析器指向名为`play.dtd`的 DTD 文件。如果激活了 DTD 验证，则将检查正在解析的 XML 文件的结构是否符合`play.dtd`中提供的结构。）

    `<!DOCTYPE PLAY SYSTEM "play.dtd">`

    不要忘记保存修改，但保持文件打开，因为稍后会再次需要它。

1.  返回`data/rich_iii.xml`并修改第 18 行中角色"KING EDWARD The Fourth"的标记。

    将开始和结束标记从`<PERSONA>`和`</PERSONA>`更改为`<PERSON>`和`</PERSON>`。现在第 18 行应该是这样的：

    `18:<PERSON>KING EDWARD The Fourth</PERSON>`

    再次，不要忘记保存修改，并保持文件打开。

1.  运行带有 DTD 验证的`SAXLocalNameCount`程序。

    这次，运行程序时将看到不同的错误：

    ```java
    java sax/SAXLocalNameCount -dtd data/rich_iii.xml
    Exception in thread "main" org.xml.sax.SAXException: 
    Error: URI=file:data/rich_iii.xml 
    Line=26: Element type "PERSON" must be declared.

    ```

    在这里，您可以看到解析器反对的一个未包含在 DTD `data/play.dtd`中的元素。

1.  在`data/rich_iii.xml`中更正"KING EDWARD The Fourth"的标记。

    将开始和结束标记恢复为原始版本，`<PERSONA>`和`</PERSONA>`。

1.  在`data/rich_iii.xml`中，从第 16 行删除`<TITLE>Dramatis Personae</TITLE>`。

    再次，不要忘记保存修改。

1.  运行带有 DTD 验证的`SAXLocalNameCount`程序。

    与以前一样，您将看到另一个验证错误：

    ```java
    java sax/SAXLocalNameCount -dtd data/rich_iii.xml
    Exception in thread "main" org.xml.sax.SAXException: 
    Error: URI=file:data/rich_iii.xml 
    Line=77: The content of element type "PERSONAE" must match "(TITLE,(PERSONA|PGROUP)+)".

    ```

    通过从第 16 行删除`<TITLE>`元素，`<PERSONAE>`元素变得无效，因为它不包含 DTD 期望的`<PERSONAE>`元素的子元素。请注意，错误消息指出错误在`data/rich_iii.xml`的第 77 行，即使您从第 16 行删除了`<TITLE>`元素。这是因为`<PERSONAE>`元素的结束标记位于第 77 行，解析器只有在解析到元素结尾时才会抛出异常。

1.  在文本编辑器中打开 DTD 文件`data/play.dtd`。

    在 DTD 文件中，你可以看到 `<PERSONAE>` 元素的声明，以及可以在符合剧本 DTD 的 XML 文档中使用的所有其他元素。 `<PERSONAE>` 的声明如下所示。

    ```java
    <!ELEMENT PERSONAE (TITLE, (PERSONA | PGROUP)+)>

    ```

    正如你所看到的，`<PERSONAE>` 元素需要一个 `<TITLE>` 子元素。 管道 (`|`) 键表示 `<PERSONA>` 或 `<PGROUP>` 子元素可以包含在 `<PERSONAE>` 元素中，而在 `(PERSONA | PGROUP)` 分组后的加号 (`+`) 键表示必须包含至少一个或多个这些子元素中的任意一个。

1.  在 `<PERSONAE>` 的声明中，在 `TITLE` 后面添加一个问号 (`?`) 键。

    在 DTD 中给子元素的声明添加一个问号，使得该子元素的一个实例的存在是可选的。

    ```java
    <!ELEMENT PERSONAE (TITLE?, (PERSONA | PGROUP)+)>

    ```

    如果在元素后面添加星号 (`*`)，则可以包含零个或多个该子元素的实例。 但是，在这种情况下，在文档的某个部分中有多个标题是没有意义的。

    不要忘记保存对 `data/play.dtd` 所做的修改。

1.  运行 `SAXLocalNameCount` 程序，并激活 DTD 验证。

    ```java
    java sax/SAXLocalNameCount -dtd data/rich_iii.xml

    ```

    这次，您应该看到 `SAXLocalNameCount` 的正确输出，没有错误。

## 实验模式验证错误

前面的练习演示了使用 `SAXLocalNameCount` 验证 XML 文件与 DTD 的过程。 在这个练习中，您将使用 `SAXLocalNameCount` 验证不同的 XML 文件，同时还要针对标准 XML Schema 定义和自定义模式源文件进行验证。 同样，通过修改 XML 文件和模式，使解析器抛出错误来演示这种类型的验证。

如上所述，这些示例重用了 `SAXLocalNameCount` 程序。 可以在 不进行验证运行 SAX 解析器示例 中找到示例及其相关文件的位置。

1.  如果还没有这样做，请将 `SAXLocalNameCount.java` 文件保存在名为 `sax` 的目录中。 在文本编辑器中打开文件并进行上述更改。

1.  如果还没有这样做，请按照以下方式编译文件：

    ```java
    javac sax/SAXLocalNameCount.java

    ```

1.  将示例 XML 文件 ``personal-schema.xml`` 保存在 `data` 目录中，然后在文本编辑器中打开它。

    这是一个简单的 XML 文件，提供了一个小公司员工的姓名和联系方式。 在这个 XML 文件中，你会看到它已经与一个模式定义文件 `personal.xsd` 相关联。

    `<personnel xsi:noNamespaceSchemaLocation='personal.xsd'>`

1.  将示例 XSD Schema 文件 ``personal.xsd`` 保存在 `data` 目录中，然后在文本编辑器中打开它。

    这个模式定义了关于每个员工所需的信息种类，以便将与模式关联的 XML 文档视为有效。例如，通过检查模式定义，你可以看到每个`person`元素需要一个`name`，每个人的名字必须包括一个`family`名和一个`given`名。员工还可以选择性地拥有电子邮件地址和 URL。

1.  在`data/personal.xsd`中，将`person`元素所需的电子邮件地址最小数量从`0`更改为`1`。

    `email`元素的声明现在如下。

    `<xs:element ref="email" minOccurs='1' maxOccurs='unbounded'/>`

1.  在`data/personal-schema.xml`中，从`person`元素`one.worker`中删除`email`元素。

    Worker One 现在看起来像这样：

    ```java
    <person id="one.worker">
      <name><family>Worker</family> <given>One</given></name>
      <link manager="Big.Boss"/>
    </person>

    ```

1.  对`personal-schema.xml`运行`SAXLocalNameCount`，不进行模式验证。

    ```java
    java sax/SAXLocalNameCount data/personal-schema.xml

    ```

    `SAXLocalNameCount`通知你每个元素在`personal-schema.xml`中出现的次数。

    ```java
    Local Name "email" occurs 5 times
    Local Name "name" occurs 6 times
    Local Name "person" occurs 6 times
    Local Name "family" occurs 6 times
    Local Name "link" occurs 6 times
    Local Name "personnel" occurs 1 times
    Local Name "given" occurs 6 times

    ```

    你会发现`email`只出现了五次，而`personal-schema.xml`中有六个`person`元素。因此，因为我们将`email`元素的最小出现次数设置为每个`person`元素为 1，我们知道这个文档是无效的。然而，因为`SAXLocalNameCount`没有被告知要根据模式验证，所以没有报告错误。

1.  再次运行`SAXLocalNameCount`，这次指定`personal-schema.xml`文档应该根据`personal.xsd`模式定义进行验证。

    正如你在上面看到的使用 XML 模式验证，`SAXLocalNameCount`有一个选项可以启用模式验证。使用以下命令运行`SAXLocalNameCount`。

    ```java
    java sax/SAXLocalNameCount -xsd data/personal-schema.xml

    ```

    这次，你将看到以下错误消息。

    ```java
    Exception in thread "main" org.xml.sax.SAXException: Error: 
    URI=file:data/personal-schema.xml 
    Line=14: cvc-complex-type.2.4.a: Invalid content was found starting with 
    element 'link'. 
    One of '{email}' is expected.

    ```

1.  将`email`元素恢复到`person`元素`one.worker`。

1.  第三次运行`SAXLocalNameCount`，再次指定`personal-schema.xml`文档应该根据`personal.xsd`模式定义进行验证。

    ```java
    java sax/SAXLocalNameCount -xsd data/personal-schema.xml

    ```

    这次你将看到正确的输出，没有错误。

1.  再次在文本编辑器中打开`personal-schema.xml`。

1.  从`personnel`元素中删除模式定义`personal.xsd`的声明。

    从`personnel`元素中删除斜体代码。

    `<personnel *xsi:noNamespaceSchemaLocation='personal.xsd'/*>`

1.  再次运行`SAXLocalNameCount`，再次指定模式验证。

    ```java
    java sax/SAXLocalNameCount -xsd data/personal-schema.xml

    ```

    很明显，这不会起作用，因为尚未声明要验证 XML 文件的模式定义。你将看到以下错误。

    ```java
    Exception in thread "main" org.xml.sax.SAXException: 
    Error: URI=file:data/personal-schema.xml 
    Line=2: cvc-elt.1: Cannot find the declaration of element 'personnel'.

    ```

1.  再次运行`SAXLocalNameCount`，这次在命令行传递模式定义文件。

    ```java
    java sax/SAXLocalNameCount -xsdss data/personal.xsd data/personal-schema.xml

    ```

    这次你使用了允许指定不在应用程序中硬编码的模式定义的`SAXLocalNameCount`选项。你应该看到正确的输出。
