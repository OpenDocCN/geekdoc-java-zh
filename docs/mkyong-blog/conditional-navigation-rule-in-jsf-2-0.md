# JSF 2.0 中的条件导航规则

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/conditional-navigation-rule-in-jsf-2-0/>

JSF 2 附带了一个非常灵活的条件导航规则来解决复杂的页面导航流程，参见下面的条件导航规则示例:

## 1.JSF·佩奇

一个简单的 JSF 页面，有一个按钮从这个页面移动到支付页面。

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
    	   <h:commandButton action="payment" value="Payment" />
	</h:form>
    </h:body>
</html> 
```

## 2.受管 Bean

一个受管 bean，提供示例数据以在导航规则中执行条件检查。

```java
 import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped; 
import java.io.Serializable;

@ManagedBean
@SessionScoped
public class PaymentController implements Serializable {

	private static final long serialVersionUID = 1L;

	public boolean registerCompleted = true;
	public int orderQty = 99;

	//getter and setter methods
} 
```

## 3.条件导航规则

通常，您在“faces-config.xml”中声明简单的导航规则，如下所示:

```java
 <navigation-rule>
	<from-view-id>start.xhtml</from-view-id>
	<navigation-case>
		<from-outcome>payment</from-outcome>
		<to-view-id>payment.xhtml</to-view-id>
	</navigation-case>
</navigation-rule> 
```

在 JSF 新协议中，您可以在进入支付页面之前添加一些条件检查，请参见以下内容:

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
			<from-outcome>payment</from-outcome>
			<if>#{paymentController.orderQty &lt; 100}</if>
			<to-view-id>ordermore.xhtml</to-view-id>
		</navigation-case>
		<navigation-case>
			<from-outcome>payment</from-outcome>
			<if>#{paymentController.registerCompleted}</if>
			<to-view-id>payment.xhtml</to-view-id>
		</navigation-case>
		<navigation-case>
			<from-outcome>payment</from-outcome>
			<to-view-id>register.xhtml</to-view-id>
		</navigation-case>
	</navigation-rule>

</faces-config> 
```

这相当于下面的 Java 代码:

```java
 if (from-view-id == "start.xhtml"){

   if(from-outcome == "payment"){

      if(paymentController.orderQty < 100){
	  return "ordermore";
      }else if(paymentController.registerCompleted){
	  return "payment";
      }else{
	  return "register";
      }

   }

} 
```

代码应该足够简单明了。

**Note**
In the conditional navigation rule, the sequence of the navigation rule does affect the navigation flow, always put the highest checking priority in the top.

## 4.测试

用于测试的不同数据集:

**例 1**

```java
 public class PaymentController implements Serializable {

	public boolean registerCompleted = true;
	public int orderQty = 99;
	... 
```

当这个按钮被点击时，它达到了"**payment controller . order qty<100**"标准，并移动到" ordermore.xhtml "页面。

**例 2**

```java
 public class PaymentController implements Serializable {

	public boolean registerCompleted = true;
	public int orderQty = 200;
	... 
```

当按钮被点击时，它点击"**payment controller . register completed**"标准并移动到" payment.xhtml "页面。

**例 3**

```java
 public class PaymentController implements Serializable {

	public boolean registerCompleted = false;
	public int orderQty = 200;
	... 
```

当按钮被单击时，它没有通过所有的检查标准，并移动到“register.xhtml”页面。

## 建议

在 JSF 2.0 中，条件导航规则中没有“ **else** 标签，希望 JSF 团队能在未来的版本中包含“else”标签。举个例子，

```java
 <navigation-rule>
	<from-view-id>start.xhtml</from-view-id>
	<navigation-case>
		<from-outcome>payment</from-outcome>
		<if>#{paymentController.orderQty &lt; 100}</if>
			<to-view-id>ordermore.xhtml</to-view-id>
		<else if>#{paymentController.registerCompleted}</else if>
			<to-view-id>payment.xhtml</to-view-id>
		<else>
			<to-view-id>register.xhtml</to-view-id>
	</navigation-case>
   </navigation-rule> 
```

此外，它还应该包括多重条件检查，就像这样

```java
 <navigation-rule>
	<from-view-id>start.xhtml</from-view-id>
	<navigation-case>
		<from-outcome>payment</from-outcome>
		<if>#{paymentController.orderQty &lt; 100} && #{paymentController.xxx}</if>
			<to-view-id>ordermore.xhtml</to-view-id>
		<else if>#{paymentController.registerCompleted}</else if>
			<to-view-id>payment.xhtml</to-view-id>
		<else>
			<to-view-id>register.xhtml</to-view-id>
	</navigation-case>
   </navigation-rule> 
```

**Thought…**
JSF 2 conditional navigation rule, … quite similar with the Spring Web Flow, right? 🙂

## 下载源代码

Download it – [JSF-2-Conditional-Navigation-Example.zip](http://web.archive.org/web/20201127022901/http://www.mkyong.com/wp-content/uploads/2010/09/JSF-2-Conditional-Navigation-Example.zip) (11KB)Tags : [jsf2](http://web.archive.org/web/20201127022901/https://mkyong.com/tag/jsf2/) [navigation rule](http://web.archive.org/web/20201127022901/https://mkyong.com/tag/navigation-rule/)<input type="hidden" id="mkyong-current-postId" value="7061">

### 相关文章

*   [JSF 2.0 中的隐式导航](/web/20201127022901/https://www.mkyong.com/jsf2/implicit-navigation-in-jsf-2-0/)
*   [JSF“从-行动”导航规则示例](/web/20201127022901/https://www.mkyong.com/jsf2/jsf-form-action-navigation-rule-example/)
*   [如何在 JSF 添加全球导航规则？](/web/20201127022901/https://www.mkyong.com/jsf2/how-to-add-a-global-navigation-rule-in-jsf/)
*   [JSF 2.0 教程](/web/20201127022901/https://www.mkyong.com/tutorials/jsf-2-0-tutorials/)
*   [JSF 2 预渲染事件示例](/web/20201127022901/https://www.mkyong.com/jsf2/jsf-2-prerenderviewevent-example/)

*   [JSF 2.0 中的多组件验证器](/web/20201127022901/https://www.mkyong.com/jsf2/multi-components-validator-in-jsf-2-0/)
*   [JSF 2 多选列表框示例](/web/20201127022901/https://www.mkyong.com/jsf2/jsf-2-multiple-select-listbox-example/)
*   [JSF 2 链接、命令链接和输出链接示例](/web/20201127022901/https://www.mkyong.com/jsf2/jsf-2-link-commandlink-and-outputlink-example/)
*   [JSF 2 列表框示例](/web/20201127022901/https://www.mkyong.com/jsf2/jsf-2-listbox-example/)
*   [JSF 2 数据表示例](/web/20201127022901/https://www.mkyong.com/jsf2/jsf-2-datatable-example/)