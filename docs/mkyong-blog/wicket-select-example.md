# Wicket 选择示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/wicket-select-example/>

Wicket 扩展附带了一个“`Select`”类，用于呈现一个下拉框组件，该组件能够用< optgroup >标签对相关选项进行**分组。**

*图:<下拉框*中的>选项组

![wicket select example](img/5ffb756d7d8e867975b9cee31f7232b0.png "wicket-select-example1")

```java
 //Java 
import org.apache.wicket.extensions.markup.html.form.select.Select;
import org.apache.wicket.extensions.markup.html.form.select.SelectOption;
...
        //variable to hold the selected value from dropdown box,
        //and also make "jQuery" selected by default
        private String selected = "jQuery";

	Select languages = new Select("languages", new PropertyModel<String>(this, "selected"));
	form.add(languages);
	languages.add(new SelectOption<String>("framework1", new Model<String>("Wicket")));
	languages.add(new SelectOption<String>("framework2", new Model<String>("Spring MVC")));
	languages.add(new SelectOption<String>("framework3", new Model<String>("JSF 2.0")));
	languages.add(new SelectOption<String>("Script1", new Model<String>("jQuery")));
	languages.add(new SelectOption<String>("Script2", new Model<String>("prototype")));

//HTML for dropdown box
<select wicket:id="languages">
	<optgroup label="Frameworks">
		<option wicket:id="framework1" >Wicket (1.4.7)</option>
		<option wicket:id="framework2" >Spring MVC (3.0)</option>
		<option wicket:id="framework3" >JSF (2.0)</option>
	</optgroup>
	<optgroup label="JavaScript">
		<option wicket:id="Script1" >jQuery (1.6.1)</option>
		<option wicket:id="Script2" >prototype (1.7)</option>
	</optgroup>
</select> 
```

##  ## 1。Wicket Extension 

要使用“`Select`”标签，您需要获得“**wicket-extensions**jar。

文件:pom.xml

```java
 <project ...>

	<dependencies>

		<dependency>
			<groupId>org.apache.wicket</groupId>
			<artifactId>wicket</artifactId>
			<version>1.4.17</version>
		</dependency>

		<dependency>
			<groupId>org.apache.wicket</groupId>
			<artifactId>wicket-extensions</artifactId>
			<version>1.4.17</version>
		</dependency>

		<!-- slf4j-log4j -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.5.6</version>
		</dependency>

	</dependencies>

</project> 
```

 ## 2.Wicket 选择示例

显示下拉框的示例，使用 **< optgroup >** 标签，通过“`Select`”和“`SelectOption`”标签对相关选项进行分组。

```java
 import org.apache.wicket.PageParameters;
import org.apache.wicket.extensions.markup.html.form.select.Select;
import org.apache.wicket.extensions.markup.html.form.select.SelectOption;
import org.apache.wicket.markup.html.form.Form;
import org.apache.wicket.markup.html.panel.FeedbackPanel;
import org.apache.wicket.markup.html.WebPage;
import org.apache.wicket.model.Model;
import org.apache.wicket.model.PropertyModel;

public class SelectPage extends WebPage {

	private String selected = "jQuery";

	public SelectPage(final PageParameters parameters) {

		add(new FeedbackPanel("feedback"));

		Form<?> form = new Form<Void>("form") {
			@Override
			protected void onSubmit() {

				info("Selected language : " + selected);

			}
		};

		add(form);

		Select languages = new Select("languages", new PropertyModel<String>(
				this, "selected"));

		form.add(languages);
		languages.add(new SelectOption<String>("framework1", new Model<String>(
				"Wicket")));
		languages.add(new SelectOption<String>("framework2", new Model<String>(
				"Spring MVC")));
		languages.add(new SelectOption<String>("framework3", new Model<String>(
				"JSF 2.0")));
		languages.add(new SelectOption<String>("Script1", new Model<String>(
				"jQuery")));
		languages.add(new SelectOption<String>("Script2", new Model<String>(
				"prototype")));

	}
} 
```

## 3.Wicket HTML 页面

匹配上述 Wicket 代码的 HTML 代码。

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
	<h1>Wicket Select example</h1>

	<div wicket:id="feedback"></div>
	<form wicket:id="form">
		<p>
		<label>Select your favor language </label> 
		<br /> 

		<select wicket:id="languages">
			<optgroup label="Frameworks">
				<option wicket:id="framework1" >Wicket (1.4.7)</option>
				<option wicket:id="framework2" >Spring MVC (3.0)</option>
				<option wicket:id="framework3" >JSF (2.0)</option>
			</optgroup>
			<optgroup label="JavaScript">
				<option wicket:id="Script1" >jQuery (1.6.1)</option>
				<option wicket:id="Script2" >prototype (1.7)</option>
			</optgroup>
		</select>

		</p>
		<input type="submit" value="Display" />
	</form>

</body>
</html> 
```

## 4.演示

开始并访问—*http://localhost:8080/wicket examples/*

默认情况下选择“jQuery”。

![wicket select example](img/5ffb756d7d8e867975b9cee31f7232b0.png "wicket-select-example1")

选择“Spring MVC”并点击显示按钮。

![wicket select example](img/ff4ae1a3ab2d6a8a021dad195931f484.png "wicket-select-example2")Download it – [Wicket-Select-Example.zip](http://web.archive.org/web/20190202032227/http://www.mkyong.com/wp-content/uploads/2011/05/Wicket-Select-Example.zip) (7KB)

## 参考

1.  [Wicket Select Javadoc](http://web.archive.org/web/20190202032227/http://wicket.apache.org/apidocs/1.4/org/apache/wicket/extensions/markup/html/form/select/Select.html)
2.  [Wicket select option Javadoc](http://web.archive.org/web/20190202032227/http://wicket.apache.org/apidocs/1.4/org/apache/wicket/extensions/markup/html/form/select/SelectOption.html)
3.  [HTML optgroup 标签](http://web.archive.org/web/20190202032227/http://www.w3schools.com/tags/tag_optgroup.asp)

[wicket](http://web.archive.org/web/20190202032227/http://www.mkyong.com/tag/wicket/)![](img/9e519e48c42a1fb7372c36c8642924ba.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190202032227/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="9067">







