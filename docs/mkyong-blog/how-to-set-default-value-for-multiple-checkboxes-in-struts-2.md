# 如何在 Struts 2 中为多个复选框设置默认值

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/how-to-set-default-value-for-multiple-checkboxes-in-struts-2/>

在 Struts 2 中，可以通过 **< s:checkboxlist >** 标签创建多个同名的复选框。棘手的部分是如何在多个复选框中设置默认值。例如，带有“红色”、“黄色”、“蓝色”、“绿色”选项的复选框列表，并且您希望将“红色”和“绿色”都设置为默认选中值。

Download It – [Struts2-default-value-multiple-checkboxes-example.zip](http://web.archive.org/web/20190310100600/http://www.mkyong.com/wp-content/uploads/2010/06/Struts2-default-value-multiple-checkboxes-example.zip)

## 1.<checkboxlist>举例</checkboxlist>

一个**<s:checkbox list>T1 的例子**

```java
 <s:checkboxlist label="What's your favor color" list="colors" name="yourColor" /> 
```

产生以下 HTML 代码

```java
 <td class="tdLabel"><label for="resultAction_yourColor" class="label">
What's your favor color:</label>
</td> 
<td > 
<input type="checkbox" name="yourColor" value="red" id="yourColor-1" /> 
<label for="yourColor-1" class="checkboxLabel">red</label> 
<input type="checkbox" name="yourColor" value="yellow" id="yourColor-2" /> 
<label for="yourColor-2" class="checkboxLabel">yellow</label> 
<input type="checkbox" name="yourColor" value="blue" id="yourColor-3" /> 
<label for="yourColor-3" class="checkboxLabel">blue</label> 
<input type="checkbox" name="yourColor" value="green" id="yourColor-4" /> 
<label for="yourColor-4" class="checkboxLabel">green</label> 
<input type="hidden" id="__multiselect_resultAction_yourColor" 
 name="__multiselect_yourColor" value="" />     
</td> 
```

操作类为复选框提供颜色选项列表。

```java
 //...
public class CheckBoxListAction extends ActionSupport{

	private List<String> colors;
	private String yourColor;

	public CheckBoxListAction(){

		colors = new ArrayList<String>();
		colors.add("red");
		colors.add("yellow");
		colors.add("blue");
		colors.add("green");
	}

	public List<String> getColors() {
		return colors;
	}
	//...
} 
```

 ## 2.单一默认检查值

要将“ **red** 选项设置为默认选中值，只需在 Action 类中添加一个方法，并返回一个“ **red** 值。

```java
 //...
public class CheckBoxListAction extends ActionSupport{

	//add a new method
	public String getDefaultColor(){
		return "red";
	}
} 
```

在 **< s:checkboxlist >标签**中，添加一个 value 属性，并指向 **getDefaultColor()** 方法。

```java
 <s:checkboxlist label="What's your favor color" list="colors" 
     name="yourColor" value="defaultColor" /> 
```

Struts 2 is intelligent enough to match the “**defaultColor**” value to the correspond Java property **getDefaultColor()**.

再次运行它，默认情况下“红色”选项将被选中。

 ## 2.多个默认选中值

要将多个值“**红色**”和“**绿色**”设置为默认检查值，只需返回一个“**字符串[]** ”而不是一个“字符串”，Struts 2 就会相应地匹配它。

```java
 //...
public class CheckBoxListAction extends ActionSupport{

	//now return a String[]
	public String[] getDefaultColor(){
		return new String [] {"red", "green"};
	}
} 
```

```java
 <s:checkboxlist label="What's your favor color" list="colors" 
     name="yourColor" value="defaultColor" /> 
```

再次运行，默认勾选“**红色**”和“**绿色**”选项。

[checkbox](http://web.archive.org/web/20190310100600/http://www.mkyong.com/tag/checkbox/) [struts2](http://web.archive.org/web/20190310100600/http://www.mkyong.com/tag/struts2/)







