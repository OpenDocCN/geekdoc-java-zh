# Struts <iterate>示例</iterate>

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-logic-iterate-example/>

在 Struts 中，可以使用 **<逻辑:iterate >** 标签迭代集合。这里有两个例子:

1.  遍历列表(原始类型)
2.  遍历列表(对象)

## 1.迭代列表数组(原始类型)

用一些虚拟字符串创建一个普通列表，并存储到`HttpServletRequest`，命名为“ **listMsg** ”。

```java
 ...
public class PrintMsgAction extends Action{

	public ActionForward execute(ActionMapping mapping,ActionForm form,
		HttpServletRequest request,HttpServletResponse response) 
        throws Exception {

		List<String> listMsg = new ArrayList<String>();

		listMsg.add("Message A");
		listMsg.add("Message B");
		listMsg.add("Message C");
		listMsg.add("Message D");

		request.setAttribute("listMsg", listMsg);

		return mapping.findForward("success");
	}

} 
```

在逻辑标记中，可以使用“name”属性(listMsg)来获取列表值。

```java
 <%@taglib uri="http://struts.apache.org/tags-bean" prefix="bean"%>
<%@taglib uri="http://struts.apache.org/tags-logic" prefix="logic"%>
<html>
<head>
</head>
<body>
<h1>Struts <logic:iterate> example</h1>

<logic:iterate name="listMsg" id="listMsgId">
<p>
	List Messages <bean:write name="listMsgId"/>
</p>
</logic:iterate>

</body>
</html> 
```

 ## 2.遍历列表数组(对象)

创建一个包含少量“用户”对象的普通列表，并将其存储到`HttpServletRequest`中，命名为“**列表用户**”。

```java
 public class User{

	String username;
	String url;

    //getter and setter methods
} 
```

```java
 ... 
public class PrintMsgAction extends Action{

	public ActionForward execute(ActionMapping mapping,ActionForm form,
		HttpServletRequest request,HttpServletResponse response) 
        throws Exception {

		List<User> listUsers = new ArrayList<User>();

		listUsers.add(new User("user1", "http://www.user1.com"));
		listUsers.add(new User("user2", "http://www.user2.com"));
		listUsers.add(new User("user3", "http://www.user3.com"));
		listUsers.add(new User("user4", "http://www.user4.com"));

		request.setAttribute("listUsers", listUsers);

		return mapping.findForward("success");
	}

} 
```

在逻辑标签内部，可以使用“ **name** ”属性(listUsers)来获取列表值；而**属性**属性则显示对象属性值。

```java
 <%@taglib uri="http://struts.apache.org/tags-bean" prefix="bean"%>
<%@taglib uri="http://struts.apache.org/tags-logic" prefix="logic"%>
<html>
<head>
</head>
<body>
<h1>Struts <logic:iterate> example</h1>

<logic:iterate name="listUsers" id="listUserId">
<p>
	List Users <bean:write name="listUserId" property="username"/> , 
	<bean:write name="listUserId" property="url"/>
</p>
</logic:iterate>

</body>
</html> 
```

![struts-logic-iterate-example](img/a59c123f2b3de245131dbb2a149ad600.png "struts-logic-iterate-example") ## 下载源代码

Download it – [Struts-logic-Iterate-example.zip](http://web.archive.org/web/20190223082036/http://www.mkyong.com/wp-content/uploads/2010/04/Struts-logic-Iterate-example.zip)[struts](http://web.archive.org/web/20190223082036/http://www.mkyong.com/tag/struts/)







