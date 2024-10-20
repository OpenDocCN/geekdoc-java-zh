# JSF“从活动”导航规则示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-form-action-navigation-rule-example/>

在 JSF 导航规则中，您可能会遇到这样的情况:两个单独的操作在一个页面中返回相同的“**结果**”。在这种情况下，您可以使用“ **from-action** ”元素来区分这两种导航情况。请参见以下示例:

## 1.受管 Bean

一个受管 bean，有两个操作返回相同的结果——“成功”。

**PageController.java**

```java
 import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import java.io.Serializable;

@ManagedBean
@SessionScoped
public class PageController implements Serializable {

	private static final long serialVersionUID = 1L;

	public String processPage1(){
		return "success";
	}

	public String processPage2(){
		return "success";
	}
} 
```

## 2.JSF·佩奇

一个 JSF 页面，有两个按钮链接到上面的`PageController`的方法。

**start.xhtml**

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html">

    <h:body>
    <h2>This is start.xhtml</h2>

      <h:form>
    	<h:commandButton action="#{pageController.processPage1}" value="Page1" />
    	<h:commandButton action="#{pageController.processPage2}" value="Page2" />
      </h:form>

    </h:body>
</html> 
```

两种行动都将返回同样的“成功”结局，JSF 如何决定何去何从？

## 3.导航规则

为了解决这个问题，在“`faces-config.xml`”中定义了以下导航规则，并使用“ **from-action** ”元素来区分相同的“结果”导航案例。

**faces-config.xml**

```java
 <?xml version="1.0" encoding="UTF-8"?>
<faces-config

    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
    http://java.sun.com/xml/ns/javaee/web-facesconfig_2_0.xsd"
    version="2.0">

    <navigation-rule>
	<from-view-id>start.xhtml</from-view-id>
	<navigation-case>
		<from-action>#{pageController.processPage1}</from-action>
		<from-outcome>success</from-outcome>
		<to-view-id>page1.xhtml</to-view-id>
	</navigation-case>
	<navigation-case>
		<from-action>#{pageController.processPage2}</from-action>
		<from-outcome>success</from-outcome>
		<to-view-id>page2.xhtml</to-view-id>
	</navigation-case>
    </navigation-rule>	
</faces-config> 
```

## 4.演示

在上面的例子中，按钮是这样工作的:

1.  当点击带有**action = " # { page controller . process page 1 } "**的按钮时，将返回“成功”结果，并移动到 **page1.xhtml**
2.  当点击带有**action = " # { page controller . process page 2 } "**的按钮时，将返回“成功”结果，并移动到 **page2.xhtml**

## 下载源代码

Download it – [JSF-2-From-Action-Navigation-Example.zip](http://web.archive.org/web/20201127022918/http://www.mkyong.com/wp-content/uploads/2010/09/JSF-2-Form-Action-Navigation-Example.zip) (11KB)Tags : [jsf2](http://web.archive.org/web/20201127022918/https://mkyong.com/tag/jsf2/) [navigation rule](http://web.archive.org/web/20201127022918/https://mkyong.com/tag/navigation-rule/)<input type="hidden" id="mkyong-current-postId" value="7068">

### 相关文章

*   [JSF 2.0 中的隐式导航](/web/20201127022918/https://www.mkyong.com/jsf2/implicit-navigation-in-jsf-2-0/)
*   [JSF 2.0 中的条件导航规则](/web/20201127022918/https://www.mkyong.com/jsf2/conditional-navigation-rule-in-jsf-2-0/)
*   [如何在 JSF 添加全球导航规则？](/web/20201127022918/https://www.mkyong.com/jsf2/how-to-add-a-global-navigation-rule-in-jsf/)
*   [JSF 2.0 教程](/web/20201127022918/https://www.mkyong.com/tutorials/jsf-2-0-tutorials/)
*   [JSF 2 预渲染事件示例](/web/20201127022918/https://www.mkyong.com/jsf2/jsf-2-prerenderviewevent-example/)

*   [JSF 2.0 中的多组件验证器](/web/20201127022918/https://www.mkyong.com/jsf2/multi-components-validator-in-jsf-2-0/)
*   [JSF 2 多选列表框示例](/web/20201127022918/https://www.mkyong.com/jsf2/jsf-2-multiple-select-listbox-example/)
*   [JSF 2 链接、命令链接和输出链接示例](/web/20201127022918/https://www.mkyong.com/jsf2/jsf-2-link-commandlink-and-outputlink-example/)
*   [JSF 2 列表框示例](/web/20201127022918/https://www.mkyong.com/jsf2/jsf-2-listbox-example/)
*   [JSF 2 数据表示例](/web/20201127022918/https://www.mkyong.com/jsf2/jsf-2-datatable-example/)