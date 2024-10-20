# struts–通配符示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-wildcards-example/>

Download this example – [Struts-Wildcards-Example.zip](http://web.archive.org/web/20190826213259/http://www.mkyong.com/wp-content/uploads/2010/04/Struts-Wildcards-Example.zip)

struts 通配符可以帮助减少 struts-config.xml 文件中的重复，只要您的 Struts 项目遵循一些常规的文件结构。例如，在用户模块中，为了实现 CRUD 函数，您的 struts-config.xml 可能如下所示

## 1.没有通配符

你需要为每个列表创建四个动作映射，添加、删除和更新函数，以及大量的重复。

**struts-config.xml**

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

	<action-mappings>

	 	<action
			path="/ListUserAction"
			type="com.mkyong.common.action.UserAction"
			parameter="ListUser"
			>

			<forward name="success" path="/pages/ListUser.jsp"/>

		</action>

		<action
			path="/AddUserAction"
			type="com.mkyong.common.action.UserAction"
			parameter="AddUser"
			>

			<forward name="success" path="/pages/AddUser.jsp"/>

		</action>

		<action
			path="/EditUserAction"
			type="com.mkyong.common.action.UserAction"
			parameter="EditUser"
			>

			<forward name="success" path="/pages/EditUser.jsp"/>

		</action>

		<action
			path="/DeleteUserAction"
			type="com.mkyong.common.action.UserAction"
			parameter="DeleteUser"
			>

			<forward name="success" path="/pages/DeleteUser.jsp"/>

		</action>

	</action-mappings>

</struts-config> 
```

## 2.带通配符

使用 Struts 通配符特性，您的 struts-config.xml 可以缩减为一个操作映射。

**struts-config.xml**

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

	<action-mappings>

	 	<action
			path="/*UserAction"
			type="com.mkyong.common.action.UserAction"
			parameter="{1}User"
			>

			<forward name="success" path="/pages/{1}User.jsp"/>

		</action>

	</action-mappings>

</struts-config> 
```

让我们来看一个用例，通过*http://localhost:8080/struts example/edit user action . do*尝试访问。“ **EditUserAction.do** ”将匹配“ **/*UserAction** ”模式， ***** 匹配字符串“ **Edit** ”用 **{1}** 表示，以备后用。

在上述情况下，通配符操作映射将从

```java
 <action
		path="/*UserAction"
		type="com.mkyong.common.action.UserAction"
		parameter="{1}User"
	>

	<forward name="success" path="/pages/{1}User.jsp"/>

	</action> 
```

到

```java
 <action
		path="/EditUserAction"
		type="com.mkyong.common.action.UserAction"
		parameter="EditUser"
	>

	<forward name="success" path="/pages/EditUser.jsp"/>

	</action> 
```

## 结论

两个 struts-config.xml 示例具有相同的功能，但是在通配符支持方面重复较少。然而，**不要**在你的项目中过度使用这个通配符特性，它比普通的声明更难管理。

[struts](http://web.archive.org/web/20190826213259/https://www.mkyong.com/tag/struts/)<input type="hidden" id="mkyong-current-postId" value="4798">







