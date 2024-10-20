# 如何在 Struts 2 中自动选择下拉框值

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/how-to-auto-select-drop-down-box-value-in-struts-2/>

在 Struts 2 中，HTML 下拉框可以通过 **< s:select >** 标签呈现。要自动选择下拉框的默认值，只需在 **< s:select >** 标签中声明一个 **value** 属性，并相应地设置默认值。

## 1.Java 列表示例

为下拉框生成选择选项的 Java 列表。

```java
 //...
public class SelectAction extends ActionSupport{

	private List<String> searchEngine;
	private String yourSearchEngine;

	//set default value
	public String getDefaultSearchEngine() {
		return "yahoo.com";
	}
	public SelectAction(){	
		searchEngine = new ArrayList<String>();
		searchEngine.add("google.com");
		searchEngine.add("bing.com");
		searchEngine.add("yahoo.com");
		searchEngine.add("baidu.com");
	}
	//...
} 
```

**< s:选择>** 标签渲染 HTML 下拉框。**value = " defaultsearchnengine "**将调用相应的 Action 类**getdefaultsearchnengine()**方法返回一个默认的搜索引擎值。

```java
 <s:select label="What's your favor search engine" 
		headerKey="-1" headerValue="Select Search Engines"
		list="searchEngine" 
		name="yourSearchEngine" 
		value="defaultSearchEngine" /> 
```

在本例中，下拉框将自动选择“**yahoo.com**”作为默认选项。

 ## 2.OGNL 列表示例

通过 OGNL 表达式创建一个下拉框，直接在“**值**属性中设置默认值。

```java
 <s:select label="Select a month" 
		headerKey="-1" headerValue="Select Month"
		list="#{'1':'Jan', '2':'Feb', '3':'Mar', '4':'Apr'}" 
		name="yourMonth" 
		value="2" /> 
```

在本例中，下拉框将自动选择**“2”(Feb)**作为默认选项。

Download It – [Struts2-Select-DropDown-Box-Example.zip](http://web.archive.org/web/20190304030851/http://www.mkyong.com/wp-content/uploads/2010/06/Struts2-Select-DropDown-Box-Example.zip)[dropdown](http://web.archive.org/web/20190304030851/http://www.mkyong.com/tag/dropdown/) [struts2](http://web.archive.org/web/20190304030851/http://www.mkyong.com/tag/struts2/)![](img/c1fc6aa0c611c2b25bd7551673c477c0.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304030851/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5940">







