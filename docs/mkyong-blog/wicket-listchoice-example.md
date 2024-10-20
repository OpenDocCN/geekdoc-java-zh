# Wicket ListChoice 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/wicket-listchoice-example/>

在 Wicket 中，可以使用`ListChoice`创建一个**单选可滚动列表框**。

```java
 //Java 
import org.apache.wicket.markup.html.form.ListChoice;
...
//choices in list box
private static final List<String> FRUITS = Arrays.asList(new String[] {
		"Apple", "Orang", "Banana" });

//variable to hold the selected list box value
private String selectedFruit = "Banana";

ListChoice<String> listFruits = new ListChoice<String>("fruit",
		new PropertyModel<String>(this, "selectedFruit"), FRUITS);

//HTML for single select listbox
<select wicket:id="fruit"></select> 
```

## 1.Wicket 单选列表框示例

示例通过“ **ListChoice** ”显示一个单选可滚动列表框，并默认一个选定值。

```java
 package com.mkyong.user;

import java.util.Arrays;
import java.util.List;
import org.apache.wicket.PageParameters;
import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.form.ListChoice;
import org.apache.wicket.markup.html.panel.FeedbackPanel;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.model.PropertyModel;

public class ListChoicePage extends WebPage {

	// single list choice
	private static final List<String> FRUITS = Arrays.asList(new String[] {
			"Apple", "Orang", "Banana" });

	// Banana is selected by default
	private String selectedFruit = "Banana";

	public ListChoicePage(final PageParameters parameters) {

		add(new FeedbackPanel("feedback"));

		ListChoice<String> listFruits = new ListChoice<String>("fruit",
				new PropertyModel<String>(this, "selectedFruit"), FRUITS);

		listFruits.setMaxRows(5);

		Form<?> form = new Form<Void>("form") {
			@Override
			protected void onSubmit() {

				info("Selected Fruit : " + selectedFruit);

			}
		};

		add(form);
		form.add(listFruits);

	}
} 
```

 ## 2.Wicket HTML 页面

页来呈现单选可滚动列表。

```java
 <html>
<head>
<style>
.feedbackPanelINFO {
	color: green;
}
</style>
</head>
<body>
	<h1>Wicket ListChoice example</h1>

	<div wicket:id="feedback"></div>
	<form wicket:id="form">
		<p>
			<label>[ListChoice] Select "ONE" of your favor fruit :</label> 
			<br />
			<select wicket:id="fruit"></select>
		</p>
		<input type="submit" value="Display" />
	</form>

</body>
</html> 
```

 ## 3.演示

开始并访问—*http://localhost:8080/wicket examples/*

自动选择“香蕉”。

![wicket listbox](img/3e56975ac197e189fa64e590d0939cd0.png "wicket-listchoice-example1")

选择“香蕉”并点击显示按钮。

![wicket listbox](img/fd8c373f0a65e10f9e31c546b11c5173.png "wicket-listchoice-example2")Download it – [Wicket-ListChoice-Examples.zip](http://web.archive.org/web/20190114180521/http://www.mkyong.com/wp-content/uploads/2011/05/Wicket-ListChoice-Examples.zip) (7KB)

## 参考

1.  [Wicket ListChoice Javadoc](http://web.archive.org/web/20190114180521/http://wicket.apache.org/apidocs/1.4/org/apache/wicket/markup/html/form/ListChoice.html)

[listbox](http://web.archive.org/web/20190114180521/http://www.mkyong.com/tag/listbox/) [wicket](http://web.archive.org/web/20190114180521/http://www.mkyong.com/tag/wicket/)







