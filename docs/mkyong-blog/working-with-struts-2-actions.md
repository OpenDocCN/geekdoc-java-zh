# 使用 Struts 2 操作

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/working-with-struts-2-actions/>

在 Struts 2 中，您将花费大部分时间处理动作。action 类包含业务逻辑、检索资源包、保存数据、验证和选择应该发送回用户的查看结果页面。这是 Struts 2 的核心，所以您必须理解动作的基本概念。

## 1.行动

Struts 2 动作不强迫你实现任何接口或扩展类，它只要求你实现一个 **execute()** 方法，该方法返回一个字符串来指示哪个结果页面应该返回。

```java
 package com.mkyong.user.action;

public class LoginAction{

	//business logic
	public String execute() {
		return "success";
	}

} 
```

在 **struts.xml** 中，用**动作标签**和**类属性**配置动作类。使用**结果标签**定义哪个结果页面应该返回给用户，并使用**名称属性**定义可以用来访问这个操作类的操作名称。

```java
 <package name="user" namespace="/User" extends="struts-default">

  <action name="validateUser" class="com.mkyong.user.action.LoginAction">
	<result name="success">pages/welcome.jsp</result>
  </action>

<package> 
```

现在，您可以通过后缀来访问该操作。动作扩展。

```java
 http://localhost:8080/Struts2Example/User/validateUser.action 
```

The default .action is configurable, just change the “[struts.action.extension](http://web.archive.org/web/20190225092953/http://www.mkyong.com/struts2/how-to-remove-the-action-suffix-extension-in-struts-2/)” value to suit your need. ## 2.可选操作界面

Struts 2 自带可选的动作接口(**com . open symphony . xwork 2 . action**)。通过实现这个接口，它带来了一些方便的好处，参见源代码:

```java
 package com.opensymphony.xwork2;

public interface Action {

    public static final String SUCCESS = "success";

    public static final String NONE = "none";

    public static final String ERROR = "error";

    public static final String INPUT = "input";

    public static final String LOGIN = "login";

    public String execute() throws Exception;

} 
```

这个接口非常简单，有 5 个常用的常量:**成功、错误、无、输入和逻辑**。现在 action 类可以直接使用常量值了。

```java
 package com.mkyong.user.action;

import com.opensymphony.xwork2.Action;

public class LoginAction{

	//business logic
	public String execute() {
		return SUCCESS;
	}

} 
```

I don’t understand why many Struts developers like to implement this Action interface, it better to extend the ActionSupport. ## 3.行动支持

Support class, a common practice to provide default implementations of interfaces.

action support(**com . open symphony . xwork 2 . action support**)，一个非常强大和方便的类，它提供了一些重要接口的默认实现:

```java
 public class ActionSupport implements Action, Validateable, 
   ValidationAware, TextProvider, LocaleProvider, Serializable {
 ...
} 
```

**ActionSupport** 类让您能够:

**1。验证**–声明了一个 validate()方法并将验证代码放入其中。
2**。文本本地化**–使用 GetText()方法从资源包中获取消息。

```java
 package com.mkyong.user.action;

import com.opensymphony.xwork2.ActionSupport;

public class LoginAction extends ActionSupport{

	private String username;
	private String password;

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	//business logic
	public String execute() {

		return "SUCCESS";

	}

        //simple validation
	public void validate(){
		if("".equals(getUsername())){
			addFieldError("username", getText("username.required"));
		}
		if("".equals(getPassword())){
			addFieldError("password", getText("password.required"));
		}
	}
} 
```

In most cases, you should extends this class for the ready convenient features, unless you have reason not to. This is also a very good learning class to understand how to do the implementation of some of the important Struts 2 interfaces.

## 4.动作注释

Struts 2 对注释有很好的支持，你可以去掉 XML 文件，在你的 action 类中用 **@action** 替换。

```java
 package com.mkyong.user.action;

import org.apache.struts2.convention.annotation.Action;
import org.apache.struts2.convention.annotation.Namespace;
import org.apache.struts2.convention.annotation.Result;
import org.apache.struts2.convention.annotation.ResultPath;

import com.opensymphony.xwork2.ActionSupport;

@Namespace("/User")
@ResultPath(value="/")
public class ValidateUserAction extends ActionSupport{

	@Action(value="Welcome", results={
		@Result(name="success",location="pages/welcome_user.jsp")
	})
	public String execute() {

		return SUCCESS;

	}
} 
```

If you want to know more about the Struts 2 annotation, please download this [Struts 2 annotation example](http://web.archive.org/web/20190225092953/http://www.mkyong.com/struts2/struts-2-hello-world-annotation-example/) for practice.

## 结论

不用动脑筋，只是扩展了 **ActionSupport** 类，它适合大多数情况。

[struts2](http://web.archive.org/web/20190225092953/http://www.mkyong.com/tag/struts2/)







