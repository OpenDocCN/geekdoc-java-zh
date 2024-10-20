# JSF 2.0 中的资源(图书馆)

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/resources-library-in-jsf-2-0/>

在 JSF 2.0 中，你所有的网络资源文件，如 css、图片或 JavaScript，都应该放在你的网络应用程序根目录下的“ **resources** 文件夹中(与“`WEB-INF`”在同一文件夹级别)。

“资源”文件夹下的**子文件夹**被认为是“**库**或者“**项目主题**”，以后你可以引用那些带有`library`属性的“资源”。这个新的 JSF 资源管理机制非常有用，它允许开发者通过“主题/库”或“版本控制”来轻松地改变网络资源。

参见以下示例:

图 1-0:一个 JSF2 项目文件夹结构的例子。

![jsf2 resources example](img/8a9ddac6d466bfd6d7860f129e07b1d2.png "jsf2-resources")

## 1.正常示例

下面是一些在 JSF 2.0 中使用“**资源**和“**库**的例子。

1.  Include CSS file – `h:outputStylesheet`

    ```java
     <h:outputStylesheet library="theme1" name="css/style.css" /> 
    ```

    HTML 输出…

    ```java
     <link type="text/css" rel="stylesheet" 
       href="/JavaServerFaces/faces/javax.faces.resource/css/style.css?ln=theme1" /> 
    ```

2.  Display images – `h:graphicImage`

    ```java
     <h:graphicImage library="theme1" name="img/sofa.png" /> 
    ```

    HTML 输出…

    ```java
     <img src="/JavaServerFaces/faces/javax.faces.resource/img/sofa.png?ln=theme1" /> 
    ```

3.  Include JavaScript – `h:outputScript`

    ```java
     <h:outputScript library="theme1" name="js/hello.js" /> 
    ```

    HTML 输出…

    ```java
     <script type="text/javascript" 
       src="/JavaServerFaces/faces/javax.faces.resource/js/hello.js?ln=theme1"> 
    ```

## 2.版本控制示例

参照*图 1-0* ，在“**库**文件夹下创建一个与 regex `\d+(_\d+)*`匹配的“**版本**文件夹，默认的 JSF `[ResourceHandler](http://web.archive.org/web/20221024080629/https://docs.oracle.com/javaee/6/api/javax/faces/application/ResourceHandler.html)`会一直得到最高版本显示。

*P.S 假设你的项目是图 1-0 结构*

包含 CSS 文件-`h:outputStylesheet`

```java
 <h:outputStylesheet library="default" name="css/style.css" /> 
```

由于“**默认**”主题包含版本“ **1_0** ”和“ **2_0** ”，JSF 总是会从最高版本获取资源，并将版本追加到资源的末尾。

查看 HTML 输出:

```java
 <link type="text/css" rel="stylesheet" 
   href="/JavaServerFaces/faces/javax.faces.resource/css/style.css?ln=default&amp;v=2_0" /> 
```

**Version is optional**
The version folder is optional, if you don’t have versioning, just omit it, like “newTheme” in Figure 1-0.

## 谢谢

感谢 [BalusC](http://web.archive.org/web/20221024080629/https://balusc.blogspot.com/) 对[的评论、指导和纠正](http://web.archive.org/web/20221024080629/http://www.mkyong.com/jsf2/resources-library-in-jsf-2-0/#comment-85218)，并为我之前误导的指导道歉。

#### 参考资料

1.  [What is JSF resource pool used for and how should it be used?](http://web.archive.org/web/20221024080629/https://stackoverflow.com/questions/11988415/what-is-the-jsf-resource-library-for-and-how-should-it-be-used)
2.  [JSF 资源处理器 JavaDoc](http://web.archive.org/web/20221024080629/https://docs.oracle.com/javaee/6/api/javax/faces/application/ResourceHandler.html)
3.  [ JSF 2.0 New Function Preview Series (Part 2.1): Resource

<input type="hidden" id="mkyong-current-postId" value="7190">