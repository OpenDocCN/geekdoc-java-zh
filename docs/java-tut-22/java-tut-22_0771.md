# 使用 XML Schema 进行验证

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/dom/validating.html`](https://docs.oracle.com/javase/tutorial/jaxp/dom/validating.html)

本节介绍了 XML Schema 验证的过程。虽然完整讨论 XML Schema 超出了本教程的范围，但本节向您展示了使用 XML Schema 定义验证 XML 文档的步骤。（要了解更多关于 XML Schema 的信息，您可以查看在线教程，[XML Schema Part 0: Primer](http://www.w3.org/TR/xmlschema-0/)。在本节结束时，您还将学习如何使用 XML Schema 定义来验证包含来自多个命名空间的元素的文档。

## 验证过程概述

要在 XML 文档中通知验证错误，必须满足以下条件：

+   必须配置工厂，并设置适当的错误处理程序。

+   文档必须与至少一个模式相关联，可能还有更多。

## 配置`DocumentBuilder`工厂

在配置工厂时，首先定义将在其中使用的常量是很有帮助的。这些常量与在使用 XML Schema 进行 SAX 解析时定义的常量相同，并且它们在`DOMEcho`示例程序的开头声明。

```java
static final String JAXP_SCHEMA_LANGUAGE =
    "http://java.sun.com/xml/jaxp/properties/schemaLanguage";
static final String W3C_XML_SCHEMA =
    "http://www.w3.org/2001/XMLSchema";

```

接下来，您配置`DocumentBuilderFactory`以生成一个支持命名空间的、使用 XML Schema 的验证解析器。这是通过在已创建的`DocumentBuilderFactory`实例`dbf`上调用`setValidating`方法来完成的，该实例是在实例化工厂中创建的。

```java
// ...

dbf.setNamespaceAware(true);
dbf.setValidating(dtdValidate || xsdValidate);

if (xsdValidate) {
    try {
        dbf.setAttribute(JAXP_SCHEMA_LANGUAGE, W3C_XML_SCHEMA);
    }
    catch (IllegalArgumentException x) {
        System.err.println("Error: JAXP DocumentBuilderFactory attribute " 
                           + "not recognized: " + JAXP_SCHEMA_LANGUAGE);
        System.err.println("Check to see if parser conforms to JAXP spec.");
        System.exit(1);
    }
}

// ...

```

因为符合 JAXP 的解析器默认情况下不支持命名空间，所以必须设置模式验证属性才能正常工作。您还设置了一个工厂属性来指定要使用的解析器语言。（另一方面，对于 SAX 解析，您在工厂生成的解析器上设置一个属性）。

## 将文档与模式关联

现在程序已准备好使用 XML Schema 定义进行验证，只需确保 XML 文档与（至少）一个相关联即可。有两种方法可以实现这一点：

+   在 XML 文档中有模式声明时

+   通过指定应用程序中要使用的模式

* * *

**注意 -** 当应用程序指定要使用的模式时，它会覆盖文档中的任何模式声明。

* * *

要在文档中指定模式定义，您可以创建如下 XML：

`<*documentRoot* xsi:noNamespaceSchemaLocation='*YourSchemaDefinition.xsd*'> [...]`

第一个属性定义了 XML 命名空间（`xmlns`）前缀`xsi`，代表“XML Schema 实例”。第二行指定了要用于文档中没有命名空间前缀的元素的模式-也就是，通常在任何简单、不复杂的 XML 文档中定义的元素。（您将看到如何处理多个命名空间在下一节中。）

您还可以在应用程序中指定模式文件，这是`DOMEcho`的情况。

```java
static final String JAXP_SCHEMA_SOURCE =
    "http://java.sun.com/xml/jaxp/properties/schemaSource";

// ...

dbf.setValidating(dtdValidate || xsdValidate);
if (xsdValidate) {
    // ...    
}

if (schemaSource != null) {
    dbf.setAttribute(JAXP_SCHEMA_SOURCE, new File(schemaSource));
}

```

在这里，您也有机制可以指定多个模式。我们将在下面看一下。

## 使用多个命名空间进行验证

命名空间允许您在同一文档中组合服务不同目的的元素，而无需担心重叠的名称。

* * *

**注意 -** 本节讨论的材料也适用于使用 SAX 解析器进行验证。您在这里看到它，是因为您已经学到足够多关于命名空间的知识，才能理解讨论的内容。

* * *

举个例子，考虑一个 XML 数据集，用于跟踪人员数据。数据集可能包括来自税务申报表和雇员入职表的信息，两个元素在各自的模式中都命名为`form`。

如果为税务命名空间定义了一个前缀，并为雇佣命名空间定义了另一个前缀，则人员数据可能包括以下部分。

```java
<employee id="...">
  <name>....</name>
  <tax:form>
     ...w2 tax form data...
  </tax:form>
  <hiring:form>
     ...employment history, etc....
  </hiring:form>
</employee>

```

`tax:form`元素的内容显然与`hiring:form`元素的内容不同，并且必须进行不同的验证。

还要注意，在此示例中存在一个默认命名空间，未限定元素名称`employee`和`name`属于该命名空间。为了使文档得到正确验证，必须声明该命名空间的模式，以及`tax`和`hiring`命名空间的模式。

* * *

**注意 -** 默认命名空间实际上是一个特定的命名空间。它被定义为“没有名称的命名空间”。因此，您不能简单地将一个命名空间用作本周的默认命名空间，然后将另一个命名空间用作以后的默认命名空间。这个“无名命名空间”（或“空命名空间”）就像数字零一样。它没有任何值（没有名称），但它仍然被精确定义。因此，具有名称的命名空间永远不能用作默认命名空间。

* * *

解析时，只要已声明这些模式，数据集中的每个元素都将根据相应的模式进行验证。同样，这些模式可以作为 XML 数据集的一部分或在程序中声明。（也可以混合声明。总的来说，最好将所有声明放在一起。）

## 在 XML 数据集中声明模式

要声明用于上述示例中数据集的模式，XML 代码将类似于以下内容。

```java
<documentRoot
  xmlns:xsi=
  "http://www.w3.org/2001/XMLSchema-instance"
  xsi:noNamespaceSchemaLocation=
    "employeeDatabase.xsd"
  xsi:schemaLocation=
  "http://www.irs.gov.example.com/ 
   fullpath/w2TaxForm.xsd
   http://www.ourcompany.example.com/ 
   relpath/hiringForm.xsd"
  xmlns:tax=
    "http://www.irs.gov.example.com/"
  xmlns:hiring=
    "http://www.ourcompany.example.com/"
>

```

`noNamespaceSchemaLocation`声明是您之前见过的内容，最后两个条目也是如此，它们定义了命名空间前缀`tax`和`hiring`。新的是中间的条目，它定义了文档中引用的每个命名空间要使用的模式的位置。

`xsi:schemaLocation`声明由条目对组成，其中每对中的第一个条目是指定命名空间的完全限定 URI，第二个条目包含模式定义的完整路径或相对路径。一般来说，建议使用完全限定路径。这样，模式只会存在一份副本。

请注意，在定义模式位置时不能使用命名空间前缀。`xsi:schemaLocation`声明只能理解命名空间名称，而不能理解前缀。

## 在应用程序中声明模式

要在应用程序中声明等效的模式，代码看起来类似于以下内容。

```java
static final String employeeSchema = "employeeDatabase.xsd";
static final String taxSchema = "w2TaxForm.xsd";
static final String hiringSchema = "hiringForm.xsd";

static final String[] schemas = {
    employeeSchema,
    taxSchema, 
    hiringSchema,
};

static final String JAXP_SCHEMA_SOURCE =
    "http://java.sun.com/xml/jaxp/properties/schemaSource";

// ...

DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance()

// ...

factory.setAttribute(JAXP_SCHEMA_SOURCE, schemas);

```

在这里，指向模式定义（`.xsd`文件）的字符串数组作为参数传递给`factory.setAttribute`方法。请注意与声明要用作 XML 数据集一部分时的区别。

+   默认（未命名）模式没有特殊声明。

+   你不需要指定命名空间名称。相反，你只需要给出指向`.xsd`文件的指针。

为了进行命名空间分配，解析器读取`.xsd`文件，并在其中找到它们适用于的目标命名空间的名称。因为文件是用 URI 指定的，所以解析器可以使用`EntityResolver`（如果已定义）来找到模式的本地副本。

如果模式定义没有定义目标命名空间，则适用于默认（未命名或 null）命名空间。因此，在我们的示例中，你会期望在模式中看到这些目标命名空间声明：

+   指向模式的 URI 的字符串

+   具有模式内容的`InputStream`

+   一个 SAX `InputSource`

+   一个文件

+   一组对象，每个对象都是这里定义的类型之一

只有当模式语言具有在运行时组装模式的能力时，才能使用对象数组。此外，当传递对象数组时，具有相同命名空间的两个模式是不允许的。

## 运行带有模式验证的`DOMEcho`示例

要运行带有模式验证的`DOMEcho`示例，请按照以下步骤进行。

1.  **导航到`samples`目录。**`% cd *install-dir*`/jaxp-1_4_2-`*release-date*`/samples`.`

1.  **编译示例类，使用刚刚设置的类路径。**`% javac dom/*`

1.  **在 XML 文件上运行`DOMEcho`程序，指定模式验证。**

    在`data`目录中选择一个 XML 文件，并使用指定的`-xsd`选项在其上运行`DOMEcho`程序。在这里，我们选择在文件`personal-schema.xml`上运行程序。

    `% java dom/DOMEcho -xsd data/personal-schema.xml`

    正如你在配置工厂中看到的，`-xsd`选项告诉`DOMEcho`对`personal-schema.xml`文件中定义的 XML 模式进行验证。在这种情况下，模式是文件`personal.xsd`，也位于`sample/data`目录中。

1.  **在文本编辑器中打开`personal-schema.xml`并删除模式声明。**

    从开头的`<personnel>`标签中删除以下内容。

    `xsi:noNamespaceSchemaLocation='personal.xsd'`

    不要忘记保存文件。

1.  **再次运行`DOMEcho`，这次指定`-xsd`选项。** `% java dom/DOMEcho -xsd data/personal-schema.xml`

    这次，你将看到一连串的错误。

1.  **再次运行`DOMEcho`，这次指定`-xsdss`选项并指定模式定义文件。**

    正如你在配置工厂中看到的，`-xsdss`选项告诉`DOMEcho`在程序运行时执行针对指定的 XML 模式定义的验证。再次使用文件`personal.xsd`。

    `% java dom/DOMEcho -xsdss data/personal.xsd data/personal-schema.xml`

    你将看到与之前相同的输出，这意味着 XML 文件已成功根据模式进行验证。
