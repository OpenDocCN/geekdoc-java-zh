# JSF 2.0 中的复合组件

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/composite-components-in-jsf-2-0/>

从 JSF 2.0 开始，创建一个可重用的组件变得非常容易，被称为**复合组件**。在本教程中，我们将向您展示如何创建一个简单的复合组件(存储为“ *register.xhtml* ”)，这是一个用户注册表单，包括姓名和电子邮件文本字段(`h:inputText`)和一个提交按钮(`h:commandButton`)。此外，我们还向您展示如何使用它。

## 创建一个复合组件

下面是创建复合组件的步骤:

freestar.config.enabled_slots.push({ placementName: "mkyong_incontent_1", slotId: "mkyong_incontent_1" });

## 1.复合名称空间

创建一个**。xhtml** 文件，并声明了复合名称空间。

```java
 <html    
      //...
      xmlns:composite="http://java.sun.com/jsf/composite">
</html> 
```

## 2.接口和实施

使用复合标签`composite:interface`、`composite:attribute`和`composite:implementation`，定义复合组件的内容。举个例子，

```java
 <html    
      //...
      xmlns:composite="http://java.sun.com/jsf/composite">

	  <composite:interface>
			<composite:attribute name="anything" />
	  </composite:interface>

	  <composite:implementation>
			#{cc.attrs.anything}
	  </composite:implementation>
</html> 
```

`composite:interface`标签用于声明可配置的值，这些值向使用它的开发人员公开。而`composite:implementation`标签声明了所有的 XHTML 标记，也就是复合组件的内容，在`composite:implementation`标签里面，你可以用表达式`#{cc.attrs.attributeName}`访问`composite:interface`属性。

## 3.资源文件夹

把复合组件(“。xhtml "文件)放到 JSF 的资源文件夹中，见图 1:

图 1:这个例子的目录结构。



![jsf2-composite-component-folder](img/4385a7cb15e9d3883e476c161a5d624b.png "jsf2-composite-component-folder")

在本例中，您将“ *register.xhtml* ”复合组件放入名为“mkyong”的文件夹中。

## 4.完整示例

做完了，让我们来看一个完整的“register.xhtml”的例子。

*文件:register.xhtml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      xmlns:composite="http://java.sun.com/jsf/composite"
      >

    <composite:interface>

    	<composite:attribute name="nameLable" />
    	<composite:attribute name="nameValue" />
    	<composite:attribute name="emailLable" />
    	<composite:attribute name="emailValue" />

   	<composite:attribute name="registerButtonText" />
    	<composite:attribute name="registerButtonAction" 
    		method-signature="java.lang.String action()" />

    </composite:interface>

    <composite:implementation>

	<h:form>

		<h:message for="textPanel" style="color:red;" />

		<h:panelGrid columns="2" id="textPanel">

			#{cc.attrs.nameLable} : 
			<h:inputText id="name" value="#{cc.attrs.nameValue}" />

			#{cc.attrs.emailLable} : 
			<h:inputText id="email" value="#{cc.attrs.emailValue}" />

		</h:panelGrid>

		<h:commandButton action="#{cc.attrs.registerButtonAction}" 
			value="#{cc.attrs.registerButtonText}"
		/>

	</h:form>

    </composite:implementation>

</html> 
```

## 使用复合组件

您刚刚创建了一个复合组件“register.xhtml ”,现在我们将向您展示如何使用它。

## 1.复合组件访问路径

参考上面的图 1；“register.xhtml”文件在“mkyong”文件夹下。以下是您访问它的方式:

```java
 <html    
      ///...
      xmlns:mkyong="http://java.sun.com/jsf/composite/mkyong"
      >
	  <mkyong:register />
</html> 
```

**http://java.sun.com/jsf/composite/folder-name-in-resources-folder**
The folder name of the composite components is defined the component access path, for example, if you put your “register.xhtml” file under folder named “abc”, then you should access it like this :

```java
 <html    
      ///...
      xmlns:mkyong="http://java.sun.com/jsf/composite/abc"
      >
	  <mkyong:register />
</html> 
```

## 2.完整示例

让我们看一个完整的例子来展示“register.xhtml”复合组件的用法。

*文件:default.xhtml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:mkyong="http://java.sun.com/jsf/composite/mkyong"
      >

    <h:body>

    	<h1>Composite Components in JSF 2.0</h1>

	<mkyong:register 
		nameLable="Name" 
		nameValue="#{user.name}" 
		emailLable="E-mail" 
		emailValue="#{user.email}"

		registerButtonText="Register" 
		registerButtonAction="#{user.registerAction}"
	 />

    </h:body>

</html> 
```

您可以通过暴露的属性将硬编码值或支持方法或属性传递到复合组件中，当提交表单时，JSF 将自动完成所有支持 bean 绑定。

对那些感兴趣的人来说，这是“用户”管理或支持的 bean。

```java
 package com.mkyong;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;

@ManagedBean(name="user")
@SessionScoped
public class UserBean{

	public String name;
	public String email;

	//getter and setter methods for name and email

	public String registerAction(){
		return "result";
	}
} 
```

## 演示

这是结果。

URL:*http://localhost:8080/Java server faces/default . XHTML*



![jsf2-composite-component-example](img/a552db042284c25ce862aa398703d12d.png "jsf2-composite-component-example")

## 下载源代码

Download It – [JSF-2-Composite-Components-Example.zip](http://web.archive.org/web/20210305085017/http://www.mkyong.com/wp-content/uploads/2010/11/JSF-2-Composite-Components-Example.zip) (11KB)

## 参考

1.  [JSF 2 组合:接口 JavaDoc](http://web.archive.org/web/20210305085017/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/composite/interface.html)
2.  [JSF 新组合:实现 JavaDoc](http://web.archive.org/web/20210305085017/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/composite/implementation.html)
3.  [JSF 2 复合:属性 JavaDoc](http://web.archive.org/web/20210305085017/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/composite/attribute.html)

Tags : [component](http://web.archive.org/web/20210305085017/https://mkyong.com/tag/component/) [jsf2](http://web.archive.org/web/20210305085017/https://mkyong.com/tag/jsf2/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="7712">