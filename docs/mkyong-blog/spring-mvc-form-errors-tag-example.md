# Spring MVC 表单错误标签示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-form-errors-tag-example/>

在 Spring MVC 中，字段错误消息由与控制器相关联的验证器生成，您可以使用 **< form:errors / >** 标签在默认的 HTML“**span**”标签中呈现这些字段错误消息。举个例子，

## 1.验证器

验证器检查“**用户名**字段，如果为空，从资源包向控制器返回“**必需.用户名**错误消息。

```java
 //...
public class TextBoxValidator implements Validator{
	@Override
	public void validate(Object target, Errors errors) {
		ValidationUtils.rejectIfEmptyOrWhitespace(
			errors, "username", "required.username");
	}
}
/*** required.username = username is required! ***/ 
```

 ## 2.显示字段错误

然后，您可以使用 **<表单:errors / >** 来呈现与“**用户名**字段相关联的错误消息。

```java
 <form:errors path="userName" cssClass="error" /> 
```

它将使用一个默认的“ **span** ”元素来呈现和封装错误消息，该元素包含一个 CSS 类“ **error** ”。

```java
 <span id="username.errors" class="error">username is required!</span> 
```

**Note**

1.  path = " * "–显示与任何字段相关的所有错误消息。
2.  path = " username "–仅显示与“username”字段相关的错误消息。

 ## 3.自定义输出元素

出于某些原因，比如 CSS 格式化的目的，您可能需要用不同的元素来包含错误消息，而不是默认的“ **span** 标签。为此，只需在“**元素**属性中指定 prefer 元素:

```java
 <form:errors path="userName" cssClass="error" element="div" /> 
```

现在，它用一个“ **div** ”元素呈现并封装错误消息，该元素包含一个“**错误**”的 CSS 类。

```java
 <div id="username.errors" class="error">username is required!</div> 
```

## 4.演示

![SpringMVC-TextBox-Example-2](img/e2d8b1e1accc5f28d994e176721fa951.png "SpringMVC-TextBox-Example-2")

## 下载源代码

Download it – [SpringMVCForm-TextBox-Example.zip](http://web.archive.org/web/20190225100238/http://www.mkyong.com/wp-content/uploads/2010/08/SpringMVCForm-TextBox-Example.zip) (9KB)[form handling](http://web.archive.org/web/20190225100238/http://www.mkyong.com/tag/form-handling/) [spring mvc](http://web.archive.org/web/20190225100238/http://www.mkyong.com/tag/spring-mvc/)







