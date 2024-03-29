# 何时使用 DOM

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/dom/when.html`](https://docs.oracle.com/javase/tutorial/jaxp/dom/when.html)

文档对象模型标准首先是为文档（例如文章和书籍）设计的。此外，JAXP 1.4.2 实现支持 XML Schema，这对于任何特定应用程序都可能是一个重要考虑因素。

另一方面，如果您处理简单的数据结构，且 XML Schema 不是您计划的重要部分，那么您可能会发现更适合您目的的是 JDOM 或 dom4j 等更面向对象的标准之一。

从一开始，DOM 旨在是与语言无关的。由于它是为诸如 C 和 Perl 之类的语言设计的，DOM 并没有利用 Java 的面向对象特性。这一事实，加上文档和数据之间的区别，也有助于解释处理 DOM 与处理 JDOM 或 dom4j 结构之间的差异。

在本节中，我们将研究这些标准背后的模型之间的差异，以帮助您选择最适合您应用程序的标准。

## 文档与数据

DOM 中使用的文档模型与 JDOM 或 dom4j 中使用的数据模型之间的主要差异在于：

+   存在于层次结构中的节点类型

+   混合内容的能力

主要是数据层次结构中的“节点”构成的差异主要导致了使用这两种模型进行编程的差异。然而，与其他任何因素相比，混合内容的能力最能解释标准如何定义节点的差异。因此，我们首先来看一下 DOM 的混合内容模型。

## 混合内容模型

在 DOM 层次结构中，文本和元素可以自由混合。这种结构称为 DOM 模型中的混合内容。

文档中经常出现混合内容。例如，假设您想要表示这种结构：

`<sentence>这是一个<bold>重要</bold>的想法。</sentence>`

DOM 节点的层次结构可能如下所示，其中每行代表一个节点：

```java
ELEMENT: sentence
   + TEXT: This is an
   + ELEMENT: bold
     + TEXT: important
   + TEXT: idea.

```

请注意，sentence 元素包含文本，然后是一个子元素，然后是额外的文本。文本和元素的混合定义了混合内容模型。

## 节点类型

为了提供混合内容的能力，DOM 节点本质上非常简单。在上述示例中，第一个元素的“内容”（其值）只是标识它是什么类型的节点。

第一次使用 DOM 的用户通常会被这个事实搞糊涂。在导航到`<sentence>`节点后，他们要求节点的“内容”，并期望得到一些有用的东西。相反，他们只能找到元素的名称，`sentence`。

* * *

**注意 -** DOM 节点 API 定义了`nodeValue()`、`nodeType()`和`nodeName()`方法。对于第一个元素节点，`nodeName()`返回`sentence`，而`nodeValue()`返回 null。对于第一个文本节点，`nodeName()`返回`#text`，而`nodeValue()`返回“`This is an`”。重要的一点是，元素的**值**与其**内容**不同。

* * *

在上面的例子中，询问“句子”的“文本”是什么意思？根据您的应用程序，以下任何一种都可能是合理的：

+   这是一个

+   这是一个想法。

+   这是一个重要的想法。

+   这是一个<bold>重要</bold>的想法。

## 一个更简单的模型

使用 DOM，您可以自由创建所需的语义。但是，您还需要进行必要的处理以实现这些语义。另一方面，像 JDOM 和 dom4j 这样的标准使得执行简单任务变得更容易，因为层次结构中的每个节点都是一个对象。

尽管 JDOM 和 dom4j 允许元素具有混合内容，但它们并非主要设计用于这种情况。相反，它们针对的是 XML 结构包含数据的应用程序。

数据结构中的元素通常只包含文本或其他元素，而不是两者兼有。例如，这里是代表简单地址簿的一些 XML：

```java
<addressbook>
    <entry>
        <name>Fred</name>
        <email>fred@home</email>
    </entry>
      ...
</addressbook>

```

* * *

**注意 -** 对于像这样非常简单的 XML 数据结构，您还可以使用内置在 Java 平台 1.4 版本中的正则表达式包（`java.util.regex`）。

* * *

在 JDOM 和 dom4j 中，当您导航到包含文本的元素后，您可以调用诸如`text()`之类的方法来获取其内容。但是，在处理 DOM 时，您必须检查子元素列表以“组合”节点的文本，就像您之前看到的那样 - 即使该列表只包含一个项目（TEXT 节点）。

因此，对于简单的数据结构，比如地址簿，您可以通过使用 JDOM 或 dom4j 来节省一些工作量。即使数据在技术上是“混合的”，但在给定节点中始终只有一个（且仅有一个）文本段落时，使用其中一个模型可能是有意义的。

这是一个这种结构的示例，也可以很容易地在 JDOM 或 dom4j 中处理：

```java
<addressbook>
    <entry>Fred
        <email>fred@home</email>
    </entry>
      ...
</addressbook>

```

在这里，每个条目都有一些标识性文本，后面跟着其他元素。有了这种结构，程序可以导航到一个条目，调用`text()`来找出它属于谁，并在正确的节点处处理`<email>`子元素。

## 增加复杂性

但是，为了全面了解在搜索或操作 DOM 时需要执行的处理类型，了解 DOM 可能包含的节点类型是很重要的。

这里有一个说明这一点的示例。这是这些数据的表示：

```java
<sentence>
    The &projectName; <![CDATA[<i>project</i>]]> is
    <?editor: red><bold>important</bold><?editor: normal>.
</sentence>

```

这个句子包含一个**实体引用** - 指向在其他地方定义的实体的指针。在这种情况下，实体包含项目的名称。示例还包含一个 CDATA 部分（未解释的数据，类似于 HTML 中的 `<pre>` 数据）以及**处理指令**（`<?...?>`），在这种情况下告诉编辑器在呈现文本时使用的颜色。

这是该数据的 DOM 结构。它代表了一个健壮应用程序应该准备处理的结构类型：

```java
+ ELEMENT: sentence
       + TEXT: The
       + ENTITY REF: projectName
        + COMMENT: 
        The latest name we are using
        + TEXT: Eagle
       + CDATA: <i>project</i>
       + TEXT: is
       + PI: editor: red
       + ELEMENT: bold
          + TEXT: important
       + PI: editor: normal

```

这个例子描述了 DOM 中可能出现的节点类型。尽管你的应用程序可能大部分时间都能忽略它们，但一个真正健壮的实现需要识别和处理每一个节点。

类似地，导航到一个节点的过程涉及处理子元素，忽略你不感兴趣的元素并检查你感兴趣的元素，直到找到你感兴趣的节点。

一个处理固定、内部生成数据的程序可以承担简化假设：处理指令、注释、CDATA 节点和实体引用在数据结构中不存在。但是真正健壮的应用程序，尤其是处理来自外部世界的各种数据的应用程序，必须准备处理所有可能的 XML 实体。

（一个“简单”的应用程序只能在输入数据包含它所期望的简化 XML 结构时工作。但是没有验证机制来确保更复杂的结构不存在。毕竟，XML 的设计目的就是允许它们存在。）

为了更加健壮，DOM 应用程序必须做到以下几点：

1.  在搜索元素时：

    1.  忽略注释、属性和处理指令。

    1.  允许子元素不按预期顺序出现的可能性。

    1.  如果不进行验证，则跳过包含可忽略空格的 TEXT 节点。

1.  在提取节点的文本时：

    1.  从 CDATA 节点以及文本节点提取文本。

    1.  在收集文本时忽略注释、属性和处理指令。

    1.  如果遇到实体引用节点或另一个元素节点，则递归（即对所有子节点应用文本提取过程）。

当然，许多应用程序不必担心这些事情，因为它们看到的数据类型将受到严格控制。但如果数据可能来自各种外部来源，那么应用程序可能需要考虑这些可能性。

执行这些功能所需的代码在本课程的末尾的 搜索节点 和 获取节点内容 中给出。现在，目标只是确定 DOM 是否适合你的应用程序。

## 选择你的模型

正如您所见，当您使用 DOM 时，即使是从节点获取文本这样的简单操作也需要一些编程。因此，如果您的程序处理简单的数据结构，那么 JDOM、dom4j，甚至 1.4 版本的正则表达式包（`java.util.regex`）可能更适合您的需求。

另一方面，对于完整的文档和复杂的应用程序，DOM 为您提供了很大的灵活性。如果需要使用 XML Schema，那么再次选择 DOM 是明智之举 - 至少目前是这样。

如果您在开发的应用程序中处理文档和数据，那么 DOM 可能仍然是您最佳选择。毕竟，一旦编写了用于检查和处理 DOM 结构的代码，就很容易为特定目的定制它。因此，选择在 DOM 中执行所有操作意味着您只需处理一组 API，而不是两组。

此外，DOM 标准是内存中文档模型的规范标准。它功能强大且稳健，并且有许多实现。这对许多大型安装来说是一个重要的决策因素，特别是对于需要尽量减少由 API 更改造成的成本的大型应用程序。

最后，即使通讯录中的文本今天可能不允许粗体、斜体、颜色和字体大小，但将来您可能会希望处理这些内容。因为 DOM 能处理几乎任何您提出的要求，选择 DOM 可以更轻松地使您的应用程序具备未来的可扩展性。
