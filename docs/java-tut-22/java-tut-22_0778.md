# 使用 XSLT 转换 XML 数据

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/xslt/transformingXML.html`](https://docs.oracle.com/javase/tutorial/jaxp/xslt/transformingXML.html)

可以使用可扩展样式表语言转换（XSLT）API 来实现多种目的。例如，通过一个足够智能的样式表，你可以从 XML 数据生成 PDF 或 PostScript 输出。但通常情况下，XSLT 用于生成格式化的 HTML 输出，或者创建数据的另一种 XML 表示。

在这一部分中，使用 XSLT 转换将 XML 输入数据转换为 HTML 输出。

* * *

**注意 -** [XSLT 规范](http://www.w3.org/TR/xslt20/)庞大而复杂，因此本教程只能触及表面。它将为您提供一些背景知识，以便您理解简单的 XSLT 处理任务，但不会详细讨论如何编写 XSLT 转换，而是集中于如何使用 JAXP 的 XSLT 转换 API。要深入了解 XSLT，请参考一本好的参考手册，例如 Michael Kay 的*XSLT 2.0 和 XPath 2.0：程序员参考*（Wrox，2008）。

* * *

## 定义一个简单的文档类型

首先定义一个非常简单的文档类型，可用于撰写文章。我们的`article`文档将包含这些结构标签：

+   `<TITLE>`: 文章的标题

+   `<SECT>`: 一个包含标题和主体的节

+   `<PARA>`: 一个段落

+   `<LIST>`: 一个列表

+   `<ITEM>`: 列表中的一个条目

+   `<NOTE>`: 一个与主文本分隔开的旁注

这种结构稍微不同寻常的地方在于，我们不会为节标题创建单独的元素标签。这样的元素通常被创建用于区分标题文本（及其包含的任何标签）和节的主体（即标题下面的任何结构元素）。

相反，我们将允许标题与节的主体无缝融合。这种安排增加了样式表的一些复杂性，但它将让我们有机会探索 XSLT 的模板选择机制。它也符合我们对文档结构的直觉期望，即标题文本直接后跟结构元素，这种安排可以简化基于大纲的编辑。

* * *

**注意 -** 这种结构不容易验证，因为 XML 的混合内容模型允许在节的任何位置出现文本，而我们希望将文本和内联元素限制在只出现在节主体的第一个结构元素之前。基于断言的验证器可以做到，但大多数其他模式机制无法。因此，我们将放弃为文档类型定义 DTD。

* * *

在这种结构中，部分可以嵌套。嵌套的深度将决定用于部分标题的 HTML 格式（例如，`h1`或`h2`）。使用普通的`SECT`标签（而不是编号的部分）在面向大纲的编辑中也很有用，因为它允许您随意移动部分，而无需担心更改任何受影响部分的编号。

对于列表，我们将使用一个类型属性来指定列表条目是无序的（带项目符号）、alpha（用小写字母符号）、ALPHA（用大写字母编号）还是编号的。

我们还将允许一些内联标签来改变文本的外观。

+   `<B>`：粗体

+   `<I>`：斜体

+   `<U>`：下划线

+   `<DEF>`：定义

+   `<LINK>`：链接到 URL

* * *

**注意 -** 内联标签不会生成换行符，因此内联标签引起的样式更改不会影响页面上文本的流动（尽管它会影响该文本的外观）。另一方面，结构标签划分了文本的新段落，因此至少总是会生成一个换行符，除了其他格式更改。

* * *

`<DEF>`标签将用于文本中定义的术语。这些术语将以斜体显示，就像它们在文档中通常显示的那样。但是在 XML 中使用特殊标签将允许索引程序找到这些定义，并将它们添加到索引中，以及标题中的关键字。例如，在前面的注释中，内联标签和结构标签的定义可以用`<DEF>`标签标记以供将来索引。

最后，`LINK`标签有两个目的。首先，它将让我们创建到 URL 的链接，而无需两次放置 URL；因此我们可以编写`<link>http//...</link>`而不是`<a href="http//...">http//...</a>`。当然，我们还希望允许一个看起来像`<link target="...">...name...</link>`的形式。这导致了`<link>`标签的第二个原因。它将让我们有机会在 XSLT 中使用条件表达式。

* * *

**注意 -** 尽管文章结构非常简单（仅由十一个标签组成），但它引发了足够多有趣的问题，让我们对 XSLT 的基本功能有了很好的了解。但是我们仍然会留下大部分规范未触及的领域。在还有什么其他 XSLT 可以做？中，我们将指出我们跳过的主要功能。

* * *

## 创建一个测试文档

在这里，您将使用嵌套的`<SECT>`元素、一些`<PARA>`元素、一个`<NOTE>`元素、一个`<LINK>`和一个`<LIST type="unordered">`创建一个简单的测试文档。这个想法是创建一个包含各种元素的文档，以便我们可以探索更有趣的翻译机制。

* * *

**注意 -** 本节讨论的代码位于`article1.xml`中，在您解压`XSLT 示例`到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录后，可以在`xslt/data`目录中找到。

* * *

要创建测试文档，请创建一个名为`article.xml`的文件，并输入以下 XML 数据。

```java
<?xml version="1.0"?>
<ARTICLE>
   <TITLE>A Sample Article</TITLE>
   <SECT>The First Major Section
      <PARA>This section will introduce a subsection.</PARA>
      <SECT>The Subsection Heading
         <PARA>This is the text of the subsection.
         </PARA>
      </SECT>
   </SECT>
</ARTICLE>

```

请注意，在 XML 文件中，子部分完全包含在主要部分中。（而在 HTML 中，标题不包含部分的正文）。结果是一个在纯文本形式下更难编辑的大纲结构，像这样，但在面向大纲的编辑器中更容易编辑。

某一天，如果有一个理解内联标记（如`<B>`和`<I>`）的树形 XML 编辑器，那么就可以以大纲形式编辑这种文章，而不需要复杂的样式表。（这样的编辑器将允许作者专注于文章的结构，将布局留到后期）。在这样的编辑器中，文章片段将看起来像这样：

```java
<ARTICLE> 
 <TITLE>A Sample Article 
  <SECT>The First Major Section 
   <PARA>This section will 
            introduce a subsection.
    <SECT>The Subheading 
     <PARA>This is the text of the subsection. 
         Note that ...

```

* * *

**注意 -** 目前存在树形结构编辑器，但它们将内联标记（如`<B>`和`<I>`）与结构标记一样处理，这可能会使“大纲”有点难以阅读。

* * *

## 编写 XSLT 转换

现在是时候开始编写一个 XSLT 转换，将转换 XML 文章并在 HTML 中呈现出来。

* * *

**注意 -** 本节讨论的代码位于`article1a.xsl`中，在将`XSLT 示例`解压缩到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录后，在`xslt/data`目录中找到。

* * *

从创建一个普通的 XML 文档开始：

```java
<?xml version="1.0" encoding="ISO-8859-1"?>

```

然后添加以下突出显示的行以创建一个 XSL 样式表：

```java
<?xml version="1.0" encoding="ISO-8859-1"?>
<xsl:stylesheet 
 xmlns:xsl=
    "http://www.w3.org/1999/XSL/Transform" 
 version="1.0"
 >
 **</xsl:stylesheet>** 
```

**现在设置它以生成兼容 HTML 的输出。

```java
<xsl:stylesheet 
[...]

   >
<xsl:output method="html"/>

[...]

</xsl:stylesheet>

```

我们将在本节稍后详细讨论该条目的原因。现在，请注意，如果你想输出除了格式良好的 XML 之外的任何内容，那么你将需要一个像所示的`<xsl:output>`标记，指定`text`或`html`。（默认值为`xml`）。

* * *

**注意 -** 当你指定 XML 输出时，你可以添加缩进属性以生成漂亮的缩进 XML 输出。规范如下：`<xsl:output method="xml" indent="yes"/>`。

* * *

## 处理基本结构元素

你将开始填写样式表，处理用于创建目录的元素：根元素、标题元素和标题。你还将处理测试文档中定义的`PARA`元素。

* * *

**注意 -** 如果你在第一次阅读时跳过了讨论 XPath 寻址机制的部分，XPath 工作原理，现在是回去复习那部分的好时机。

* * *

从添加处理根元素的主要指令开始：

```java
 <xsl:template match="/">
      <html><body>
         <xsl:apply-templates/>
      </body></html>
   </xsl:template>

</xsl:stylesheet>

```

新的 XSL 命令以粗体显示。（请注意，它们在`xsl`命名空间中定义）。指令`<xsl:apply-templates>`处理当前节点的子节点。在这种情况下，当前节点是根节点。

尽管这个例子很简单，但它阐明了许多重要的概念，因此值得彻底理解。第一个概念是样式表包含许多模板，用`<xsl:template>`标签定义。每个模板包含一个匹配属性，该属性使用 XPath 工作原理中描述的 XPath 寻址机制来选择模板将应用于的元素。

在模板内部，不以`xsl: namespace`前缀开头的标签只是简单地复制。随后的换行符和空格也会被复制，这有助于使生成的输出可读。

* * *

**注意 -** 当没有换行符时，通常会忽略空格。要在这种情况下包含输出中的空格，或者包含其他文本，您可以使用`<xsl:text>`标签。基本上，XSLT 样式表期望处理标签。因此，它看到的一切都需要是`<xsl:..>`标签、其他标签或空格。

* * *

在这种情况下，非 XSL 标签是 HTML 标签。因此，当匹配根标签时，XSLT 输出 HTML 起始标签，处理适用于根节点子元素的任何模板，然后输出 HTML 结束标签。

## 处理`<TITLE>`元素

接下来，添加一个模板来处理文章标题：

```java
 <xsl:template match="/ARTICLE/TITLE">
 <h1 align="center"> 
    <xsl:apply-templates/> </h1>
 </xsl:template>

</xsl:stylesheet>

```

在这种情况下，您指定了到 TITLE 元素的完整路径，并输出一些 HTML 以使标题文本成为一个大的、居中的标题。在这种情况下，`apply-templates`标签确保如果标题包含任何内联标签，如斜体、链接或下划线，它们也将被处理。

更重要的是，`apply-templates`指令导致标题文本被处理。与 DOM 数据模型一样，XSLT 数据模型基于包含在元素节点中的文本节点的概念（这些元素节点又可以包含在其他元素节点中，依此类推）。这种分层结构构成了源树。还有一个包含输出的结果树。

XSLT 通过将源树转换为结果树来工作。要可视化 XSLT 操作的结果，了解这些树的结构及其内容是有帮助的。（有关此主题的更多信息，请参见 XSLT/XPath 数据模型）。

## 处理标题

要继续处理基本结构元素，添加一个模板来处理顶级标题：

```java
 <xsl:template match=
    "/ARTICLE/SECT">
 <h2> <xsl:apply-templates
 select="text()|B|I|U|DEF|LINK"/> 
</h2>
 <xsl:apply-templates select=
    "SECT|PARA|LIST|NOTE"/>
 </xsl:template>

</xsl:stylesheet>

```

在这里，您指定了到最顶层`SECT`元素的路径。但这次，您使用`select`属性在两个阶段应用模板。在第一阶段，您使用 XPath 的`text()`函数选择文本节点，以及加粗和斜体等内联标签（竖线（`|`）用于匹配多个项目：文本或加粗标签或斜体标签等）。在第二阶段，您选择文件中包含的其他结构元素，如章节、段落、列表和注释。

使用 select 属性可以让您将文本和内联元素放在`<h2>...</h2>`标签之间，同时确保随后处理该部分中的所有结构标签。换句话说，您确保 XML 文档中标题的嵌套不会反映在 HTML 格式中，这是对 HTML 输出很重要的区别。

一般来说，使用 select 子句可以让您将所有模板应用于当前上下文中可用信息的子集。作为另一个示例，此模板选择当前节点的所有属性：

```java
<xsl:apply-templates select="@*"/></attributes>

```

接下来，添加几乎相同的模板来处理嵌套一级更深的子标题：

```java
 <xsl:template match=
    "/ARTICLE/SECT/SECT">
 <h3> <xsl:apply-templates
 select="text()|B|I|U|DEF|LINK"/> 
</h3>
 <xsl:apply-templates select=
    "SECT|PARA|LIST|NOTE"/>
 </xsl:template>

</xsl:stylesheet>

```

## 生成运行时消息

您也可以为更深层的标题添加模板，但在某个时候必须停止，即使只是因为 HTML 只支持五级标题。在本例中，您将在两级部分标题处停止。但是，如果 XML 输入恰好包含第三级，您将希望向用户提供错误消息。本节将向您展示如何做到这一点。

* * *

**注意 -** 我们可以继续处理更深层的`SECT`元素，通过使用表达式`/SECT/SECT//SECT`选择它们。`//`选择任何`SECT`元素，深度由 XPath 寻址机制定义。但我们将利用这个机会来玩弄消息传递。

* * *

添加以下模板以在遇到嵌套过深的部分时生成错误：

```java
 <xsl:template match=
    "/ARTICLE/SECT/SECT/SECT">
 <xsl:message terminate="yes">
 Error: Sections can only be nested 2 deep.
 </xsl:message>
 </xsl:template>

</xsl:stylesheet>

```

`terminate="yes"`子句导致转换过程在生成消息后停止。如果没有它，处理仍然会继续，该部分中的所有内容都将被忽略。

作为额外的练习，您可以扩展样式表以处理嵌套高达四个部分的部分，生成`<h2>...<h5>`标签。在任何嵌套五级深的部分上生成错误。

最后，通过添加一个模板来处理`PARA`标签来完成样式表：

```java
 <xsl:template match="PARA">
 <p><xsl:apply-templates/></p>
 </xsl:template>
</xsl:stylesheet>

```

## 编写基本程序

现在，您将修改使用 XSLT 的程序，以原样回显 XML 文件，并将其修改为使用您的样式表。

* * *

**注意 -** 本节讨论的代码位于`Stylizer.java`中，在将`XSLT examples`解压缩到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录后，可以在`xslt/data`中找到`stylizer1a.html`。 

* * *

`Stylizer`示例改编自`TransformationApp02`，该示例解析 XML 文件并写入`System.out`。这两个程序之间的主要区别如下所述。

首先，`Stylizer`在创建`Transformer`对象时使用样式表。

```java
// ...
import javax.xml.transform.dom.DOMSource; 
import javax.xml.transform.stream.StreamSource; 
import javax.xml.transform.stream.StreamResult; 
// ... 

public class Stylizer {
    // ...
    public static void main (String argv[]) {
        // ...
        try {
            File stylesheet = new File(argv[0]);
            File datafile = new File(argv[1]);

            DocumentBuilder builder = factory.newDocumentBuilder();
            document = builder.parse(datafile);
            // ...
            StreamSource stylesource = new StreamSource(stylesheet); 
            Transformer transformer = Factory.newTransformer(stylesource);
        }
    }
}

```

此代码使用文件创建`StreamSource`对象，然后将源对象传递给工厂类以获取转换器。

* * *

**注意 -** 通过消除`DOMSource`类，您可以简化代码。不要为 XML 文件创建`DOMSource`对象，而是为其创建`StreamSource`对象，以及为样式表创建`StreamSource`对象。

* * *

### 运行`Stylizer`示例

1.  **导航至`samples`目录。**

    ```java
    % cd *install-dir*/jaxp-1_4_2-*release-date*/samples.

    ```

1.  **点击此链接下载 XSLT 示例，并将其解压缩到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录中。**

1.  **导航至`xslt`目录。**

    ```java
    cd xslt

    ```

1.  **编译`Stylizer`示例。**

    输入以下命令：

    ```java
    % javac Stylizer.java

    ```

1.  **使用样式表`article1a.xsl`在`article1.xml`上运行`Stylizer`示例。**

    ```java
    % java Stylizer data/article1a.xsl  data/article1.xml

    ```

    您将看到以下输出：

    ```java
    <html>
    <body>

    <h1 align="center">A Sample Article</h1>
    <h2>The First Major Section

    </h2>
    <p>This section will introduce a subsection.</p>
    <h3>The Subsection Heading

    </h3>
    <p>This is the text of the subsection.
    </p>

    </body>
    </html>

    ```

    此时，输出中有相当多的多余空格。在下一节中，您将看到如何消除大部分空格。

## 修剪空白

请记住，当您查看 DOM 的结构时，有许多只包含可忽略空格的文本节点。输出中的大部分多余空格来自这些节点。幸运的是，XSL 提供了一种消除它们的方法。（有关节点结构的更多信息，请参阅 XSLT/XPath 数据模型。）

* * *

**注意 -** 本节讨论的样式表在解压缩`XSLT 示例`到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录后，在`xslt/data`目录中找到的`article1b.xsl`中。结果是在`xslt/data`中找到的`stylizer1b.html`。

* * *

要去除一些多余的空白，将以下突出显示的行添加到样式表中。

```java
<xsl:stylesheet ...
   >
<xsl:output method="html"/> 
<xsl:strip-space elements="SECT"/>

[...]

```

此指令告诉 XSL 删除`SECT`元素下只包含空格的文本节点。不包含空格以外文本的节点不会受影响，也不会影响其他类型的节点。

### 运行带有修剪空白的`Stylizer`示例

1.  **导航至`samples`目录。**

    ```java
    % cd *install-dir*/jaxp-1_4_2-*release-date*/samples.

    ```

1.  **点击此链接下载 XSLT 示例，并将其解压缩到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录中。**

1.  **导航至`xslt`目录。**

    ```java
    cd xslt

    ```

1.  **编译`Stylizer`示例。**

    输入以下命令：

    ```java
    % javac Stylizer.java

    ```

1.  **使用样式表`article1b.xsl`在`article1.xml`上运行`Stylizer`示例。**

    ```java
    % java Stylizer 
      data/article1b.xsl  
      data/article1.xml

    ```

    您将看到以下输出：

    ```java
    <html>
    <body>

    <h1 align="center">A Sample Article</h1>

    <h2>The First Major Section
       </h2>
    <p>This section will introduce a subsection.</p>
    <h3>The Subsection Heading
          </h3>
    <p>This is the text of the subsection.
          </p>

    </body>
    </html>

    ```

    这是一个相当大的改进。标题后仍然有换行符和空格，但这些来自 XML 的编写方式：

    ```java
    <SECT>The First Major Section
    ____<PARA>This section will introduce a subsection.</PARA>
    ^^^^

    ```

    在这里，您可以看到节标题以换行符和缩进空格结束，然后是 PARA 条目开始。这不是一个大问题，因为处理 HTML 的浏览器通常会压缩和忽略多余的空格。但我们还有一个格式化工具可供使用。

## 移除最后的空白

* * *

**注意 -** 本节讨论的样式表在`article1c.xsl`中，解压`XSLT examples`到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录后，可以在`xslt/data`目录中找到。结果是在`xslt/data`中找到的`stylizer1c.html`。

* * *

最后那一点空白空间通过将以下内容添加到样式表中来处理：

```java
   <xsl:template match="text()">
 <xsl:value-of select="normalize-space()"/>
 </xsl:template>

</xsl:stylesheet>

```

使用此样式表运行`Stylizer`将删除所有剩余的空白空间。

### 使用所有空白空间修剪的`Stylizer`示例运行

1.  **导航到`samples`目录。**

    ```java
    % cd *install-dir*/jaxp-1_4_2-*release-date*/samples.

    ```

1.  **点击此链接下载 XSLT 示例并将其解压缩到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录中。**

1.  **导航到`xslt`目录。**

    ```java
    cd xslt

    ```

1.  **编译`Stylizer`示例。**

    输入以下命令：

    ```java
    % javac Stylizer.java

    ```

1.  **使用样式表`article1c.xsl`在`article1.xml`上运行`Stylizer`示例。**

    ```java
    % java Stylizer 
      data/article1c.xsl  
      data/article1.xml

    ```

    输出现在看起来像这样：

    ```java
    <html>
    <body>
    <h1 align="center">A Sample Article
    </h1>
    <h2>The First Major Section</h2>
    <p>This section will introduce a subsection.
    </p>
    <h3>The Subsection Heading</h3>
    <p>This is the text of the subsection.
    </p>
    </body>
    </html>

    ```

    那已经好多了。当然，如果能缩进会更好，但事实证明这比预期的要困难一些。以下是一些可能的攻击途径，以及困难所在：

    缩进选项

    不幸的是，可以应用于 XML 输出的`indent="yes"`选项对 HTML 输出不可用。即使该选项可用，也不会有帮助，因为 HTML 元素很少嵌套！尽管 HTML 源经常缩进以显示暗示的结构，但 HTML 标记本身并未以创建真实结构的方式嵌套。

    缩进变量

    `<xsl:text>`函数允许您添加任何文本，包括空格。因此，它可能被用于输出缩进空间。问题在于变化缩进空间的数量。XSLT 变量似乎是一个好主意，但在这里不起作用。原因是当您在模板中为变量赋值时，该值仅在该模板内（在编译时静态地）被知晓。即使变量在全局范围内定义，分配的值也不会以一种使其在运行时动态地被其他模板知晓的方式存储。当`<apply-templates/>`调用其他模板时，这些模板不知道其他地方设置的任何变量设置。

    参数化模板

    使用参数化模板是修改模板行为的另一种方法。但确定要传递的缩进空间量作为参数仍然是问题的关键。

    目前，似乎没有任何好的方法来控制 HTML 格式化输出的缩进。如果您需要将 HTML 显示或编辑为纯文本，那将会很不方便。但如果您在 XML 表单上进行编辑，仅在浏览器中显示 HTML 版本，则不是问题（例如，当您查看`stylizer1c.html`时，您会看到预期的结果）。

## 处理剩余的结构元素

在这一部分，您将处理 `LIST` 和 `NOTE` 元素，这些元素为文章添加了更多结构。

* * *

**注意 -** 本节描述的示例文档是 `article2.xml`，用于操作它的样式表是 `article2.xsl`。结果是 `stylizer2.html`。解压缩 `XSLT examples` 到 *install-dir*`/jaxp-1_4_2-`*release-date*`/samples` 目录后，这些文件可以在 `xslt/data` 目录中找到。

* * *

首先向示例文档添加一些测试数据：

```java
<?xml version="1.0"?>
<ARTICLE>
<TITLE>A Sample Article</TITLE>
 <SECT>The First Major Section
    ...
  </SECT>
  <SECT>The Second Major Section
  <PARA>This section adds a LIST and a NOTE.
    <PARA>Here is the LIST:
      <LIST type="ordered">
        <ITEM>Pears</ITEM>
        <ITEM>Grapes</ITEM>
      </LIST>
  </PARA>
  <PARA>And here is the NOTE:
  <NOTE>Don't forget to go to the 
           hardware store on your way
           to the grocery!
  </NOTE>
  </PARA>
 </SECT> 
</ARTICLE>

```

* * *

**注意 -** 虽然 XML 文件中的 `list` 和 `note` 包含在各自的段落中，但它们是否包含并不重要；生成的 HTML 无论如何都是相同的。但是包含它们会使它们在面向大纲的编辑器中更容易处理。

* * *

## 修改 `<PARA>` 处理

接下来，修改 `PARA` 模板以考虑我们现在允许一些结构元素嵌入到段落中的事实：

```java
<xsl:template match="PARA">
<p> <xsl:apply-templates select=
    "text()|B|I|U|DEF|LINK"/>
 </p>
 <xsl:apply-templates select=
    "PARA|LIST|NOTE"/>
</xsl:template>

```

这种修改使用了你用于节标题的相同技术。唯一的区别是 `SECT` 元素不应该出现在段落中。（但是，一个段落很容易存在于另一个段落内，例如，作为引用材料）。

## 处理 `<LIST>` 和 `<ITEM>` 元素

现在你已经准备好添加一个模板来处理 `LIST` 元素：

```java
<xsl:template match="LIST">
 <xsl:if test="@type='ordered'"> 
  <ol>
   <xsl:apply-templates/>
    </ol>
    </xsl:if>
    <xsl:if test="@type='unordered'">
     <ul>
      <xsl:apply-templates/>
     </ul>
 </xsl:if>
</xsl:template>

</xsl:stylesheet>

```

`<xsl:if>` 标签使用 `test=""` 属性来指定一个布尔条件。在这种情况下，测试值是 type 属性，并且生成的列表会根据值是有序还是无序而改变。

在这个例子中注意两个重要的事情：

+   没有 else 子句，也没有 return 或 exit 语句，因此需要两个 `<xsl:if>` 标签来覆盖两个选项。（或者可以使用 `<xsl:choose>` 标签，它提供了 case 语句功能）。

+   在属性值周围需要使用单引号。否则，XSLT 处理器会尝试将 ordered 作为 XPath 函数来解释，而不是作为字符串。

现在通过处理 ITEM 元素来完成 LIST 处理：

```java
 <xsl:template match="ITEM">
 <li><xsl:apply-templates/>
 </li>
 </xsl:template>

</xsl:stylesheet>

```

## 在样式表中排序模板

到目前为止，你应该已经了解到模板是彼此独立的，所以它们通常出现在文件中的位置并不重要。因此，从这一点开始，我们只会展示你需要添加的模板。（为了比较，它们总是添加在示例样式表的末尾）。

当两个模板可以应用于同一节点时，顺序确实很重要。在这种情况下，最后定义的模板是被找到和处理的。例如，要将缩进列表的排序更改为使用小写字母表，可以指定一个看起来像这样的模板模式：`//LIST//LIST`。在该模板中，您将使用 HTML 选项生成字母枚举，而不是数字枚举。

但这样的元素也可以通过模式`//LIST`来识别。为了确保正确处理，指定`//LIST`的模板必须出现在指定`//LIST//LIST`的模板之前。

## 处理`<NOTE>`元素

唯一剩下的结构元素是`NOTE`元素。添加以下模板来处理它。

```java
 <xsl:template match="NOTE">
 <blockquote><b>Note:</b><br/>
 <xsl:apply-templates/>
 </p></blockquote>
 </xsl:template>

</xsl:stylesheet>

```

这段代码引发了一个有趣的问题，这是由于包含`<br/>`标签导致的。为了使文件成为格式良好的 XML，必须在样式表中指定该标签为`<br/>`，但许多浏览器不识别该标签。虽然大多数浏览器识别序列`<br></br>`，但它们都将其视为段落换行而不是单个换行。

换句话说，转换必须生成一个`<br>`标签，但样式表必须指定`<br/>`。这就是我们在样式表中早期添加的特殊输出标签的主要原因：

```java
<xsl:stylesheet ... >
   <xsl:output method="html"/>
   [...]
</xsl:stylesheet>

```

该输出规范将空标签（如`<br/>`）转换为它们的 HTML 形式`<br>`，在输出时。这种转换很重要，因为大多数浏览器不识别空标签。以下是受影响的标签列表：

```java
area      frame   isindex
base      hr      link
basefont  img     meta
br        input   param
col

```

总之，默认情况下，XSLT 在输出时生成格式良好的 XML。因为 XSL 样式表本身就是格式良好的 XML，所以你不能轻易地在其中间放置`<br>`这样的标签。`<xsl:output method="html"/>`标签解决了这个问题，这样你可以在样式表中编码`<br/>`，但在输出中得到`<br>`。

指定`<xsl:output method="html"/>`的另一个主要原因是，与指定`<xsl:output method="text"/>`一样，生成的文本不会被转义。例如，如果样式表包含`<`实体引用，它将出现为生成文本中的`<`字符。另一方面，当生成 XML 时，样式表中的`<`实体引用将保持不变，因此在生成的文本中会显示为`<`。

* * *

**注意 -** 如果你希望`<`实际上作为 HTML 输出的一部分生成，你需要将其编码为`&lt;`。这个序列在输出时变为`<`，因为只有`&`被转换为`&`字符。

* * *

### 使用定义了`LIST`和`NOTE`元素的`Stylizer`示例运行

1.  **导航到`samples`目录。**

    ```java
    % cd *install-dir*/jaxp-1_4_2-*release-date*/samples.

    ```

1.  **点击此链接下载 XSLT 示例**，并将其解压缩到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录中。

1.  **导航到`xslt`目录。**

    ```java
    cd xslt

    ```

1.  **编译`Stylizer`示例。**

    输入以下命令：

    ```java
    % javac Stylizer.java

    ```

1.  **使用样式表`article2.xsl`在`article2.xml`上运行`Stylizer`示例。**

    ```java
    % java Stylizer data/article2.xsl  data/article2.xml

    ```

    这是当你现在运行程序时为第二部分生成的 HTML：

    ```java
    ...
    <h2>The Second Major Section
    </h2>
    <p>This section adds a LIST and a NOTE.
    </p>
    <p>Here is the LIST:
    </p>
    <ol>
    <li>Pears</li>
    <li>Grapes</li>
    </ol>
    <p>And here is the NOTE:
    </p>
    <blockquote>
    <b>Note:</b>
    <br>Do not forget to go to the hardware store on your way to the grocery!
    </blockquote>

    ```

## 处理内联（内容）元素

`ARTICLE`类型中唯一剩下的标签是内联标签-它们不会在输出中创建换行，而是被整合到它们所属的文本流中。

内联元素与结构元素不同，内联元素是标签内容的一部分。 如果将元素视为文档树中的节点，则每个节点都具有内容和结构。 内容由它包含的文本和内联标记组成。 结构由标签下的其他元素（结构元素）组成。

* * *

**注意 -** 本节描述的示例文档是`article3.xml`，用于操作它的样式表是`article3.xsl`。 结果是`stylizer3.html`。

* * *

首先向示例文档添加一些测试数据：

```java
<?xml version="1.0"?>
<ARTICLE>
 <TITLE>A Sample Article</TITLE>
  <SECT>The First Major Section
      [...]
  </SECT>
  <SECT>The Second Major Section
      [...]
  </SECT> 
<SECT>The <i>Third</i> 
    Major Section
 <PARA>In addition to the inline tag
    in the heading, 
 this section defines the term  
    <DEF>inline</DEF>,
 which literally means "no line break". 
 It also adds a simple link to the main page 
    for the Java platform 
(<LINK>http://java.sun.com</LINK>),
 as well as a link to the 
 <LINK target="http://java.sun.com/xml">
   XML </LINK> 
 page.
 </PARA>
 </SECT> 
</ARTICLE>

```

现在处理段落中的内联`<DEF>`元素，将它们重命名为 HTML 斜体标记：

```java
<xsl:template match="DEF">
 <i> <xsl:apply-templates/> </i> 
</xsl:template>

```

接下来，注释掉文本节点规范化。它已经达到了它的目的，现在你需要保留重要的空格：

```java
<!--  
<xsl:template match="text()">
  <xsl:value-of select="normalize-space()"/>
   </xsl:template>
-->

```

这个修改使我们不会丢失在`<I>`和`<DEF>`等标签之前的空格。 （尝试在没有此修改的情况下运行程序，看看结果）。

现在处理基本的内联 HTML 元素，如`<B>`，`<I>`和`<U>`，用于加粗，斜体和下划线。

```java
<xsl:template match="B|I|U">
 <xsl:element name="{name()}">
 <xsl:apply-templates/>
 </xsl:element> 
</xsl:template>

```

`<xsl:element>`标签允许您计算要生成的元素。 在这里，您使用当前元素的名称生成适当的内联标记。 特别要注意在`name=".."`表达式中使用大括号（`{}`）。 这些大括号导致引号内的文本被处理为 XPath 表达式，而不是被解释为字面字符串。 在这里，它们导致 XPath `name()`函数返回当前节点的名称。

大括号可以出现在属性值模板可以出现的任何地方。 （属性值模板在 XSLT 规范的第 7.6.2 节中定义，并且它们出现在模板定义的几个地方）。 在这种表达式中，大括号也可以用于引用属性的值，`{@foo}`，或元素的内容`{foo}`。

* * *

**注意 -** 您还可以使用`<xsl:attribute>`生成属性。 有关更多信息，请参阅 XSLT 规范的第 7.1.3 节。

* * *

最后剩下的元素是`LINK`标签。处理该标签的最简单方法是设置一个带有参数的命名模板：

```java
<xsl:template name="htmLink">
 <xsl:param name="dest" 
    select="UNDEFINED"/> 
 <xsl:element name="a">
 <xsl:attribute name="href">
 <xsl:value-of select=""/>
 </xsl:attribute>
 <xsl:apply-templates/> 
 </xsl:element> 
</xsl:template>

```

这个模板的主要区别在于，不是指定匹配子句，而是使用`name=""`子句为模板指定一个名称。 因此，只有在调用它时才会执行此模板。

在模板内部，还使用`<xsl:param>`标签指定一个名为`dest`的参数。 为了进行一些错误检查，您使用 select 子句为该参数指定了一个默认值`UNDEFINED`。 要在`<xsl:value-of>`标签中引用变量，您指定`.`。

`* * *

**注意 -** 请记住，引号中的条目被解释为表达式，除非它进一步被单引号括起来。 这就是为什么之前需要在`"@type='ordered'"`中使用单引号来确保 ordered 被解释为字符串。

* * *

`<xsl:element>`标签生成一个元素。之前，你可以简单地通过编写类似`<html>`的代码来指定我们想要的元素。但是在这里，你正在动态生成`<xsl:element>`标签的主体中 HTML 锚点(`<a>`)的内容。并且你正在使用`<xsl:attribute>`标签动态生成锚点的`href`属性。

模板的最后一个重要部分是`<apply-templates>`标签，它插入`LINK`元素下文本节点中的文本。如果没有它，生成的 HTML 链接中将没有文本。

接下来，添加`LINK`标签的模板，并在其中调用命名模板：

```java
<xsl:template match="LINK">
 <xsl:if test="@target">
 <!--Target attribute specified.-->
 <xsl:call-template 
    name="htmLink">
 <xsl:with-param name="dest" 
    select="@target"/> 
 </xsl:call-template>
 </xsl:if>
</xsl:template>
<xsl:template name="htmLink">

[...]

```

`test="@target"`子句在 LINK 标签中存在 target 属性时返回 true。因此，当链接的文本和为其定义的目标不同时，这个`<xsl-if>`标签会生成 HTML 链接。

`<xsl:call-template>`标签调用命名模板，而`<xsl:with-param>`使用`name`子句指定参数，并使用`select`子句指定其值。

在样式表构建过程的最后一步中，添加`<xsl-if>`标签来处理没有`target`属性的`LINK`标签。

```java
<xsl:template match="LINK">
   <xsl:if test="@target">
      [...]
   </xsl:if>

   <xsl:if test="not(@target)">
 <xsl:call-template name="htmLink">
 <xsl:with-param name="dest">
 <xsl:apply-templates/>
 </xsl:with-param>
 </xsl:call-template>
 </xsl:if>
</xsl:template>

```

`not(...)`子句反转了先前的测试（记住，没有 else 子句）。因此，当未指定目标属性时，模板的这部分被解释。这次，参数值不是来自`select`子句，而是来自`<xsl:with-param>`元素的内容。

* * *

**注意 -** 仅仅是为了明确：参数和变量（稍后在 XSLT 还能做什么？中讨论）的值可以通过`select`子句指定，让你可以使用 XPath 表达式，也可以通过元素的内容指定，让你可以使用 XSLT 标签。

* * *

在这种情况下，参数的内容是由`<xsl:apply-templates/>`标签生成的，它插入`LINK`元素下文本节点的内容。

### 运行带有内联元素定义的`Stylizer`示例

1.  **导航到`samples`目录。**

    ```java
    % cd *install-dir*/jaxp-1_4_2-*release-date*/samples.

    ```

1.  **点击此链接下载 XSLT 示例并将其解压缩到*install-dir*`/jaxp-1_4_2-`*release-date*`/samples`目录。**

1.  **导航到`xslt`目录。**

    ```java
    cd xslt

    ```

1.  **编译`Stylizer`示例。**

    输入以下命令：

    ```java
    % javac Stylizer.java

    ```

1.  **使用样式表`article3.xsl`在`article3.xml`上运行`Stylizer`示例。**

    ```java
    % java Stylizer data/article3.xsl  data/article3.xml

    ```

    现在运行程序，结果应该看起来像这样：

    ```java
    [...]
    <h2>The <i>Third</i> Major Section</h2>
    <p>In addition to the inline tag in the heading, this section  defines the term <i>inline</i>, which literally means "no line break". It also adds a simple link to the main page for the Java platform (<a href="http://java.sun.com">http://java.sun.com</a>),  as well as a link to the <a href="http://java.sun.com/xml">XML</a> page.</p>

    ```

    干得好！你现在已经将一个相当复杂的 XML 文件转换为 HTML。（尽管一开始看起来很简单，但它确实提供了很多探索的机会）。

## 打印 HTML

您现在已将 XML 文件转换为 HTML。总有一天，会有人制作出一个了解 HTML 的打印引擎，您将能够通过 Java 打印服务 API 找到并使用它。到那时，您将能够通过生成 HTML 打印任意 XML 文件。您只需设置一个样式表并使用您的浏览器。

## XSLT 还能做什么？

尽管本节内容很长，但只是触及了 XSLT 的能力表面。XSLT 规范中还有许多额外的可能性等待着您。以下是一些值得关注的事项：

`import`（第 2.6.2 节）和 `include`（第 2.6.1 节）

`rt`（第 2.6.2 节）和 include（第 2.6.1 节）使用这些语句对 XSLT 样式表进行模块化和组合。include 语句只是插入所包含文件中的任何定义。import 语句允许您用自己样式表中的定义覆盖导入文件中的定义。

`for-each` 循环（第 8 节）

遍历一系列项目并依次处理每个项目。

`choose`（case 语句）用于条件处理（第 9.2 节）

根据输入值分支到多个处理路径之一。

生成数字（第 7.7 节）

动态生成编号的章节、编号元素和数字文字。XSLT 提供三种编号模式：

+   Single：将项目编号在单个标题下，就像 HTML 中的有序列表

+   Multiple：生成多级编号，如"A.1.3"

+   Any：无论项目出现在何处，都连续编号，就像课程中的脚注。

格式化数字（第 12.3 节）

控制枚举格式，以便获得数字（`format="1"`）、大写字母（`format="A"`）、小写字母（`format="a"`）或复合数字，如"A.1"，以及适合特定国际区域设置的数字和货币金额。

排序输出（第 10 节）

按照所需的排序顺序生成输出。

基于模式的模板（第 5.7 节）

多次处理一个元素，每次在不同的“模式”中。您可以向模板添加一个模式属性，然后指定`<apply-templates mode="...">`来仅应用具有匹配模式的模板。结合`<apply-templates select="...">`属性，将基于模式的处理应用于输入数据的子集。

变量（第 11 节）

变量类似于方法参数，可以控制模板的行为。但它们并不像你想象的那样有价值。变量的值仅在定义它的当前模板或`<xsl:if>`标签（例如）的范围内才可知。你无法将一个值从一个模板传递到另一个模板，甚至无法将一个模板的封闭部分的值传递到同一模板的另一部分。

即使是“全局”变量，这些说法也是正确的。你可以在模板中更改它的值，但更改仅适用于该模板。当用于定义全局变量的表达式被评估时，该评估发生在结构的根节点的上下文中。换句话说，全局变量本质上是运行时常量。这些常量可以用于改变模板的行为，特别是与包含和导入语句结合使用时。但变量不是通用的数据管理机制。

## 变量的问题

创建一个单一模板并为链接的目标设置一个变量，而不是费力地设置一个带参数的模板并以两种不同的方式调用它，这是一个诱人的想法。这个想法是将变量设置为默认值（比如，`LINK`标签的文本），然后，如果目标属性存在，将目的地变量设置为目标属性的值。

如果这个方法行得通就好了。但问题在于变量只在定义它们的范围内才被知晓。因此，当你编写一个`<xsl:if>`标签来改变变量的值时，该值只在`<xsl:if>`标签的上下文中被知晓。一旦遇到`</xsl:if>`，对变量设置的任何更改都会丢失。

一个同样诱人的想法是用一个变量（`）来替换`text()|B|I|U|DEF|LINK`规范。但由于变量的值取决于它的定义位置，全局内联变量的值由文本节点、`<B>`节点等组成，这些节点恰好存在于根级别。换句话说，在这种情况下，这样一个变量的值是空的。**
