# JSF 2.0 中的隐式导航

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/implicit-navigation-in-jsf-2-0/>

在 JSF 1.2 中，所有的页面导航都需要在"`faces-config.xml`"文件中声明如下:

```java
 ...
<navigation-rule>
   <from-view-id>page1.xhtml</from-view-id>
   <navigation-case>
       <from-outcome>page2</from-outcome>
       <to-view-id>/page2.xhtml</to-view-id>
   </navigation-case>
</navigation-rule>
... 
```

在 JSF 2 中，它将“结果”作为页面名称，例如，导航到“page1.xhtml”，您必须将“结果”作为“page1”。这种机制被称为“**隐式导航**”，在这里你不需要声明繁琐的导航规则，而是直接把“结果”放在动作属性中，JSF 就会自动找到正确的“**视图 id** ”。

在 JSF 2 中有两种方法来实现隐式导航。

## 1.JSF 的结果页面

你可以把“结果”直接放在 JSF 页面上。

**page 1 . XHTML**–一个 JSF 页面，带有一个从当前页面移动到“page2.xhtml”的命令按钮。

```java
 <h:form>
    <h:commandButton action="page2" value="Move to page2.xhtml" />
</h:form> 
```

点击按钮后，JSF 会将动作值或结果，“ **page2** ”与“ **xhtml** ”扩展名合并，在当前的“ **page1.xhtml** 目录中找到视图名“ **page2.xhtml** ”。

## 2.受管 Bean 的结果

此外，您还可以像这样定义受管 bean 中的“结果”:

**PageController.java**

```java
 @ManagedBean
@SessionScoped
public class PageController implements Serializable {

	public String moveToPage2(){
	    return "page2"; //outcome
	}
} 
```

在 JSF 页面中，action 属性，只需使用“**方法表达式**调用方法即可。

**page1.xhtml**

```java
 <h:form>
    <h:commandButton action="#{pageController.moveToPage2}" 
	value="Move to page2.xhtml by managed bean" />
</h:form> 
```

## 重寄

默认情况下，JSF 2 是执行一个前进，而导航到另一个页面，这导致该页面的网址总是落后一个:)。例如，当您从“page1.xhtml”移动到“page2.xhtml”时，浏览器 URL 地址栏仍会显示相同的“page 1 . XHTML”URL。

为了避免这种情况，您可以通过在“outcome”字符串的末尾附加“ **faces-redirect=true** ”来告诉 JSF 使用重定向。

```java
 <h:form>
    <h:commandButton action="page2?faces-redirect=true" value="Move to page2.xhtml" />
</h:form> 
```

**Note**
For simple page navigation, this new implicit navigation is more then enough; For complex page navigation, you are still allow to declare the page flow (navigation rule) in the **faces-config.xml** file.

## 下载源代码

Download it – [JSF-2-Implicit-Navigation-Example.zip](http://web.archive.org/web/20210505153540/http://www.mkyong.com/wp-content/uploads/2010/09/JSF-2-Implicit-Navigation-Example.zip) (10KB)Tags : [jsf2](http://web.archive.org/web/20210505153540/https://mkyong.com/tag/jsf2/) [navigation rule](http://web.archive.org/web/20210505153540/https://mkyong.com/tag/navigation-rule/)<input type="hidden" id="mkyong-current-postId" value="7053">