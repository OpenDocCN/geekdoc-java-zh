# Wicket CompoundPropertyModel 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/wicket-compoundpropertymodel-example/>

在 Wicket 中， **CompoundPropertyModel** 类似于 [PropertyModel](http://web.archive.org/web/20190114230013/http://www.mkyong.com/wicket/wicket-propertymodel-example/) ，是将表单组件绑定到对象属性最常用的模型。

**Note**
For detail, read this [CompoundPropertyModel article](http://web.archive.org/web/20190114230013/https://cwiki.apache.org/WICKET/working-with-wicket-models.html#WorkingwithWicketmodels-CompoundPropertyModels)

请看下面的例子来展示如何在 Wicket 中使用 CompoundPropertyModel。

## 1.用户级

一个用户类，有两个属性——“姓名”和“年龄”。

```java
 package com.mkyong.user;

import java.io.Serializable;

public class User implements Serializable{

	private String name;
	private int age;

	//setter and getter methods

} 
```

 ## 2.CompoundPropertyModel 示例

使用“ **CompoundPropertyModel** ”将 textbox 组件绑定到“**用户**对象。

```java
 package com.mkyong.user;

import org.apache.wicket.PageParameters;
import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.form.TextField;
import org.apache.wicket.markup.html.panel.FeedbackPanel;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.model.CompoundPropertyModel;

public class UserPage extends WebPage {

	private User user = new User();

	@SuppressWarnings("serial")
	public UserPage(final PageParameters parameters) {

		add(new FeedbackPanel("feedback"));

		final TextField<String> tName = new TextField<String>("name");
		final TextField<Integer> tAge = new TextField<Integer>("age");

		Form<User> form = new Form<User>("userForm", 
                                                     new CompoundPropertyModel<User>(user)) {

			@Override
			protected void onSubmit() {

				PageParameters pageParameters = new PageParameters();
				pageParameters.add("name", user.getName());
				pageParameters.add("age", Integer.toString(user.getAge()));
				setResponsePage(SuccessPage.class, pageParameters);

			}

		};

		add(form);
		form.add(tName);
		form.add(tAge);

	}
} 
```

在这种情况下，

1.  `new TextField<String>("name")`将绑定到用户对象，**名称属性**
2.  `new TextField<Integer>("age")`绑定到用户对象，**年龄属性**

Download It – [Wicket-CompoundPropertyModel-Examples.zip](http://web.archive.org/web/20190114230013/http://www.mkyong.com/wp-content/uploads/2011/05/Wicket-CompoundPropertyModel-Examples.zip) (8KB) ## 参考

1.  [CompoundPropertyModel Javadoc](http://web.archive.org/web/20190114230013/http://wicket.apache.org/apidocs/1.4/org/apache/wicket/model/CompoundPropertyModel.html)
2.  [使用复合属性模型](http://web.archive.org/web/20190114230013/https://cwiki.apache.org/WICKET/working-with-wicket-models.html#WorkingwithWicketmodels-CompoundPropertyModels)

[wicket](http://web.archive.org/web/20190114230013/http://www.mkyong.com/tag/wicket/)







