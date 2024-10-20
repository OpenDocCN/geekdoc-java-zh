# Wicket 页面参数示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/wicket-pageparameters-example/>

在 Wicket 中，可以使用“ [PageParameters](http://web.archive.org/web/20190303050719/http://wicket.apache.org/apidocs/1.4/org/apache/wicket/PageParameters.html) ”类来存储参数值并将其传递给另一个页面。

## PageParameters 示例

参见下面的例子，它给名为“ **msg** ”的参数添加一个值，并通过`setResponsePage()`将其发送给另一个“ **SuccessPage** ”。

```java
 Form<?> form = new Form<Void>("userForm") {

		@Override
		protected void onSubmit() {

			PageParameters pageParameters = new PageParameters();
			pageParameters.add("msg", "this is parameter value");
			setResponsePage(SuccessPage.class, pageParameters);

		}
	}; 
```

在“ **SuccessPage** ”中，像这样取回参数值“msg ”:

```java
 public class SuccessPage extends WebPage {

	public SuccessPage(final PageParameters parameters) {

		String result = "";

		if(parameters.containsKey("msg")){
			result = parameters.getString("msg");
		}

	}
} 
```

[parameter](http://web.archive.org/web/20190303050719/http://www.mkyong.com/tag/parameter/) [wicket](http://web.archive.org/web/20190303050719/http://www.mkyong.com/tag/wicket/)![](img/d287b00354d437738ccdd3ff0d70ff88.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190303050719/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="8970">







