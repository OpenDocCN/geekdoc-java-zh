# Struts 2 关键属性示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-key-attribute-example/>

Download It – [Struts2-Key-Attribute-Example.zip](http://web.archive.org/web/20190304004337/http://www.mkyong.com/wp-content/uploads/2010/06/Struts2-ActionError-ActionMessage-Example.zip)

在 Struts 2 中，UI 组件中的" **key** "属性是处理本地化的一种常见方式，也是编码 UI 标签的一种非常有效的方式。参见下面两个案例:

## 1.属性文件

属性文件包含一条消息。

**global.properties**

```java
 global.username = Username 
```

 ## 2.案例 1

如果您将一个“**键**属性分配给一个文本字段。key 属性将从资源包中获取消息，并基于**默认的 xhtml text.tfl 模板**呈现它。

```java
 <s:form action="validateUser">
	<s:textfield key="global.username" />
</s:form> 
```

现在，它将从 global.properties 文件中获取“**global . Username {左侧}** ”和“**Username {右侧}** ”，并与下面的 xhtml text.tfl 模板匹配。

```java
 <td class="tdLabel">
   <label for="validateUser_{left-side}" class="label">{right-side}:</label>
</td>
<td>
   <input type="text" name="{left-side}" value="" id="validateUser_{left-side}"/>
</td> 
```

**最终 HTML**

```java
 <td class="tdLabel">
   <label for="validateUser_global_username" class="label">Username:</label>
</td>
<td>
   <input type="text" name="global.username" value="" id="validateUser_global_username"/>
</td> 
```

key 属性将以**{左侧}** 作为文本框名称和 id；**{右侧}** 为标签值。

 ## 3.案例 2

在某些情况下，您可能需要为文本框显式声明一个不同的名称。

```java
 <s:form action="validateUser">
	<s:textfield key="global.username" name="username"/>
</s:form> 
```

现在，键属性将使用“**用户名{右侧}** ”来匹配标签值，文本框名称和 id 将被显式覆盖。

**最终 HTML**

```java
 <td class="tdLabel">
   <label for="validateUser_username" class="label">Username:</label>
</td>
<td>
   <input type="text" name="username" value="" id="validateUser_username"/>
</td> 
```

The key attribute may increase your development speed and make your code more efficient, it’s worth to learn it.[struts2](http://web.archive.org/web/20190304004337/http://www.mkyong.com/tag/struts2/)







