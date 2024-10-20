# 在 Wicket 中创建自定义验证器

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/create-custom-validator-in-wicket/>

在本教程中，您将创建一个自定义密码验证器，并将其附加到密码字段。

请参见创建自定义验证程序的总结步骤:

1.实现`IValidator`。

```java
 import org.apache.wicket.validation.IValidator;

public class StrongPasswordValidator implements IValidator<String>{
	...
} 
```

2.超越`validate(IValidatable <string>validatable)`。

```java
 public class StrongPasswordValidator implements IValidator<String>{
	...
	@Override
	public void validate(IValidatable<String> validatable) {

		//get input from attached component
		final String field = validatable.getValue();

	}
} 
```

3.将自定义验证程序附加到表单组件。

```java
 public class CustomValidatorPage extends WebPage {

	public CustomValidatorPage(final PageParameters parameters) {

  	    final PasswordTextField password = new PasswordTextField("password",Model.of(""));
		//attached custom validator to password field
		password.add(new StrongPasswordValidator());

		//...
	}

} 
```

## 完整示例

参见下面的 Wicket 示例，创建一个定制的密码验证器，并在密码与预定义的模式不匹配时显示一条错误消息。

 ## 1.StrongPasswordValidator

自定义密码验证程序。

```java
 package com.mkyong.user;

import java.util.regex.Pattern;

import org.apache.wicket.validation.IValidatable;
import org.apache.wicket.validation.IValidator;
import org.apache.wicket.validation.ValidationError;

public class StrongPasswordValidator implements IValidator<String> {

	private final String PASSWORD_PATTERN 
                              = "((?=.*\\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[@#$%]).{6,20})";

	private final Pattern pattern;

	StrongPasswordValidator() {
		pattern = Pattern.compile(PASSWORD_PATTERN);
	}

	@Override
	public void validate(IValidatable<String> validatable) {

		final String password = validatable.getValue();

		// validate password
		if (pattern.matcher(password).matches() == false) {

			//Message from key "StrongPasswordValidator.not-strong-password"
			error(validatable, "not-strong-password");

		}

	}

	private void error(IValidatable<String> validatable, String errorKey) {
		ValidationError error = new ValidationError();
		error.addMessageKey(getClass().getSimpleName() + "." + errorKey);
		validatable.error(error);
	}

} 
```

*文件:package.properties*

```java
 StrongPasswordValidator.not-strong-password = Password required at least ... (omitted) 
```

 ## 2.附加到组件

将上述自定义验证程序附加到密码字段。

```java
 package com.mkyong.user;

import org.apache.wicket.PageParameters;
import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.form.PasswordTextField;
import org.apache.wicket.markup.html.panel.FeedbackPanel;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.model.Model;

public class CustomValidatorPage extends WebPage {

	public CustomValidatorPage(final PageParameters parameters) {

		add(new FeedbackPanel("feedback"));

		final PasswordTextField password 
                                = new PasswordTextField("password",Model.of(""));

		//attached custom validator to password field
		password.add(new StrongPasswordValidator());

		Form<?> form = new Form<Void>("form") {
			@Override
			protected void onSubmit() {
				info("Done");
			}
		};

		add(form);
		form.add(password);

	}

} 
```

```java
 <html>
<head>
<style>
.feedbackPanelERROR {
	color: red;
}
</style>
</head>
<body>
	<h1>Wicket custom validator example</h1>

	<div wicket:id="feedback"></div>
	<form wicket:id="form">
		<p>
			<label>Password</label>: 
                        <input wicket:id="password" type="password" size="20" />
		</p>
		        <input type="submit" value="Register" />
	</form>

</body>
</html> 
```

## 3.演示

开始并访问—*http://localhost:8080/wicket examples/*

键入弱密码，则从自定义验证程序返回并显示错误消息。

![](img/d98751f4259ef2e5d4e64aa5dc761b37.png "wicket-custom-validator-example")Download it – [Wicket-Custom-Validator-Example.zip](http://web.archive.org/web/20190310101943/http://www.mkyong.com/wp-content/uploads/2011/06/Wicket-Custom-Validator-Example.zip) (9KB)

## 参考

1.  [Wicket IValidator Javadoc](http://web.archive.org/web/20190310101943/http://wicket.apache.org/apidocs/1.4/org/apache/wicket/validation/IValidator.html)
2.  [Wicket PasswordTextField 示例](http://web.archive.org/web/20190310101943/http://www.mkyong.com/wicket/wicket-password-field-example/)
3.  [强密码的正则表达式](http://web.archive.org/web/20190310101943/http://www.mkyong.com/regular-expressions/how-to-validate-password-with-regular-expression/)

[validator](http://web.archive.org/web/20190310101943/http://www.mkyong.com/tag/validator/) [wicket](http://web.archive.org/web/20190310101943/http://www.mkyong.com/tag/wicket/)







