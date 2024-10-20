# 从 JSF 页面向后台 bean 传递参数的 4 种方法

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/4-ways-to-pass-parameter-from-jsf-page-to-backing-bean/>

据我所知，从 JSF 页面向后台 bean 传递参数值有 4 种方式:

1.  方法表达式(JSF 2.0)
2.  问:我的钱
3.  f:属性
4.  f:setPropertyActionListener

让我们一个一个地看例子:

## 1.方法表达式

从 JSF 2.0 开始，允许在方法表达式中像这样传递参数值 **#{bean.method(param)}** 。

*JSF 页……*

```java
 <h:commandButton action="#{user.editAction(delete)}" /> 
```

*背豆……*

```java
 @ManagedBean(name="user")
@SessionScoped
public class UserBean{

	public String editAction(String id) {
	  //id = "delete"
	}

} 
```

**Note**
If you are deploy JSF application in servlet container like Tomcat, make sure you include the “**el-impl-2.2.jar**” properly. For detail, please read this article – [JSF 2.0 method expression caused error in Tomcat](http://web.archive.org/web/20190213134747/http://www.mkyong.com/jsf2/how-to-pass-parameters-in-method-expression-jsf-2-0/). ## 第二，我的钱

通过`f:param`标签传递参数值，并通过 backing bean 中的请求参数取回。

*JSF 页……*

```java
 <h:commandButton action="#{user.editAction}">
	<f:param name="action" value="delete" />
</h:commandButton> 
```

*背豆……*

```java
 @ManagedBean(name="user")
@SessionScoped
public class UserBean{

	public String editAction() {

	  Map<String,String> params = 
                FacesContext.getExternalContext().getRequestParameterMap();
	  String action = params.get("action");
          //...

	}

} 
```

点击这里查看完整的 [f:param 示例](http://web.archive.org/web/20190213134747/http://www.mkyong.com/jsf2/jsf-2-param-example/)。

 ## 3.女:属性

通过`f:atribute`标签传递参数值，并通过 backing bean 中的动作监听器获取它。

*JSF 页……*

```java
 <h:commandButton action="#{user.editAction}" actionListener="#{user.attrListener}"> 
	<f:attribute name="action" value="delete" />
</h:commandButton> 
```

*背豆……*

```java
 @ManagedBean(name="user")
@SessionScoped
public class UserBean{

  String action;

  //action listener event
  public void attrListener(ActionEvent event){

	action = (String)event.getComponent().getAttributes().get("action");

  }

  public String editAction() {
	//...
  }	

} 
```

在这里可以看到完整的 [f:attribute 示例](http://web.archive.org/web/20190213134747/http://www.mkyong.com/jsf2/jsf-2-attribute-example/)。

## 4\. f:setPropertyActionListener

通过`f:setPropertyActionListener`标签传递参数值，它会将值直接设置到您的后台 bean 属性中。

*JSF 页……*

```java
 <h:commandButton action="#{user.editAction}" >
    <f:setPropertyActionListener target="#{user.action}" value="delete" />
</h:commandButton> 
```

*背豆……*

```java
 @ManagedBean(name="user")
@SessionScoped
public class UserBean{

	public String action;

	public void setAction(String action) {
		this.action = action;
	}

	public String editAction() {
	   //now action property contains "delete"
	}	

} 
```

请在此查看完整的 [f:setPropertyActionListener 示例](http://web.archive.org/web/20190213134747/http://www.mkyong.com/jsf2/jsf-2-setpropertyactionlistener-example/)。

*P.S 请分享你的想法，如果你有任何其他方法:)*

[backing bean](http://web.archive.org/web/20190213134747/http://www.mkyong.com/tag/backing-bean/) [jsf2](http://web.archive.org/web/20190213134747/http://www.mkyong.com/tag/jsf2/) [parameter](http://web.archive.org/web/20190213134747/http://www.mkyong.com/tag/parameter/)







