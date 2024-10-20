# Wicket 中的自定义 NotEqualInputValidator

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/notequalinputvalidator-in-wicket/>

Wicket 包含许多为开发人员提供的内置验证器，例如，`EqualInputValidator`，它最适合比较两个表单组件的身份。然而，Wicket 没有为不等于验证器提供任何验证器。

## NotEqualInputValidator

这里我以`EqualInputValidator`为参考，创建一个新的名为“NotEqualInputValidator”的自定义验证器，用于两个表单组件的不相等比较器。

```java
 package com.mkyong.user;

import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.form.FormComponent;
import org.apache.wicket.markup.html.form.validation.AbstractFormValidator;
import org.apache.wicket.util.lang.Objects;

public class NotEqualInputValidator extends AbstractFormValidator {

	private static final long serialVersionUID = 1L;

	/** form components to be checked. */
	private final FormComponent<?>[] components;

	/**
	 * Construct.
	 * 
	 * @param formComponent1
	 *            a form component
	 * @param formComponent2
	 *            a form component
	 */
	public NotEqualInputValidator(FormComponent<?> formComponent1,
			FormComponent<?> formComponent2) {
		if (formComponent1 == null) {
			throw new IllegalArgumentException(
					"argument formComponent1 cannot be null");
		}
		if (formComponent2 == null) {
			throw new IllegalArgumentException(
					"argument formComponent2 cannot be null");
		}
		components = new FormComponent[] { formComponent1, formComponent2 };
	}

	public FormComponent<?>[] getDependentFormComponents() {
		return components;
	}

	public void validate(Form<?> form) {
		// we have a choice to validate the type converted values or the raw
		// input values, we validate the raw input
		final FormComponent<?> formComponent1 = components[0];
		final FormComponent<?> formComponent2 = components[1];

		if (Objects.equal(formComponent1.getInput(), formComponent2.getInput())) {
			error(formComponent2);
		}
	}

} 
```

 ## 怎么用？

要使用它，只需像普通的验证器一样附加它。

```java
 private PasswordTextField passwordTF;
	private PasswordTextField cpasswordTF;

	add(new NotEqualInputValidator(oldpasswordTF,passwordTF)); 
```

**Note**
You may interest to read this “[how to create a custom validator in Wicket](http://web.archive.org/web/20190304031826/http://www.mkyong.com/wicket/create-custom-validator-in-wicket/)“.[wicket](http://web.archive.org/web/20190304031826/http://www.mkyong.com/tag/wicket/)![](img/5be3d30e38d614f03fdc1141dfa3edec.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304031826/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="1752">







