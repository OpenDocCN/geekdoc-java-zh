# Wicket 密码字段示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/wicket-password-field-example/>

Wicket 教程向您展示了如何创建两个密码字段——“**密码**和“**确认密码**”，附加一个**强密码验证器**，并将密码值传递到下一页。

```java
 //Java 
import org.apache.wicket.markup.html.form.PasswordTextField;
...
final PasswordTextField password = new PasswordTextField("password", Model.of(""));
form.add(password);

//HTML
<input wicket:id="password" type="password" size="20" /> 
```

## 1.Wicket 密码示例

呈现两个密码字段的用户页面。附加两个验证器，`PatternValidator`和`EqualPasswordInputValidator`用于密码检查。

*文件:UserPage.java*

```java
 package com.mkyong.user;

import org.apache.wicket.PageParameters;
import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.form.PasswordTextField;
import org.apache.wicket.markup.html.form.validation.EqualPasswordInputValidator;
import org.apache.wicket.markup.html.panel.FeedbackPanel;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.model.Model;
import org.apache.wicket.validation.validator.PatternValidator;

public class UserPage extends WebPage {

	//1 digit, 1 lower, 1 upper, 1 symbol "@#$%", from 6 to 20
	private final String PASSWORD_PATTERN 
                          = "((?=.*\\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[@#$%]).{6,20})";

	public UserPage(final PageParameters parameters) {

		add(new FeedbackPanel("feedback"));

		//create a password field
		final PasswordTextField password = new PasswordTextField("password",
				Model.of(""));
		//for properties file
		password.setLabel(Model.of("Password")); 

		final PasswordTextField cpassword = new PasswordTextField("cpassword",
				Model.of(""));
		cpassword.setLabel(Model.of("Confirm Password"));

		password.add(new PatternValidator(PASSWORD_PATTERN));

		Form<?> form = new Form<Void>("userForm") {
			@Override
			protected void onSubmit() {
				//get the entered password and pass to next page
				PageParameters pageParameters = new PageParameters();
				pageParameters.add("password", password.getModelObject());
				setResponsePage(SuccessPage.class, pageParameters);

			}
		};

		add(form);
		form.add(password);
		form.add(cpassword);
		form.add(new EqualPasswordInputValidator(password, cpassword));

	}
} 
```

*File : UserPage.html*

```java
 <html>
<head>
<style>
label {
	background-color: #eee;
	padding: 4px;
}

.feedbackPanelERROR {
	color: red;
}
</style>
</head>
<body>
	<h1>Wicket password Example</h1>

	<div wicket:id="feedback"></div>
	<form wicket:id="userForm">
		<p>
			<label>Password</label>: 
                        <input wicket:id="password" type="password" size="20" />
		</p>
		<p>
			<label>Confirm Password</label>: 
                        <input wicket:id="cpassword" type="password" size="20" />
		</p>
		<input type="submit" value="Register" />
	</form>

</body>
</html> 
```

 ## 2.包.属性

将 string 放在一个“ **package.properties** 中，这样它就可以在其他页面之间共享。

*文件:package.properties*

```java
 password.Required = ${label} is required
cpassword.Required = ${label} is required
password.PatternValidator = ${label} should contains at least 1 digit, ... (omitted) 
cpassword.EqualPasswordInputValidator = "${label} did not match!" 
```

 ## 3.演示

开始并访问—*http://localhost:8080/wicket examples/*

如果密码不符合正则表达式模式:

![wicket pattern error](img/f27bee8fec54d4285caf89ca0d573ae1.png "wicket-password-validate-error1")

如果密码和确认密码不匹配:

![wicket password error](img/984b752d522991698ee0d5e7c96c27ae.png "wicket-password-validate-error2")Download it – [Wicket-password-example.zip](http://web.archive.org/web/20190310100512/http://www.mkyong.com/wp-content/uploads/2011/05/Wicket-password-example.zip) (8KB)

## 参考

1.  [用正则表达式验证密码](http://web.archive.org/web/20190310100512/http://www.mkyong.com/regular-expressions/how-to-validate-password-with-regular-expression/)
2.  [Wicket PasswordTextField Javadoc](http://web.archive.org/web/20190310100512/http://wicket.apache.org/apidocs/1.4/org/apache/wicket/markup/html/form/PasswordTextField.html)

[password](http://web.archive.org/web/20190310100512/http://www.mkyong.com/tag/password/) [wicket](http://web.archive.org/web/20190310100512/http://www.mkyong.com/tag/wicket/)







