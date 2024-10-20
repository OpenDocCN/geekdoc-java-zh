# Struts DispatchAction 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-dispatchaction-example/>

DispatchAction 类(**org . Apache . struts . Actions . dispatch action**)提供了一种将所有相关函数分组到单个 action 类中的方法。这是一种有用机制，可以避免为每个函数创建单独的 action 类。

Download this Struts DispatchAction example – [Struts-DispatchAction-Example.zip](http://web.archive.org/web/20190223075601/http://www.mkyong.com/wp-content/uploads/2010/04/Struts-Localization-Example.zip)

要实现这个机制，您的 Action 类需要扩展**org . Apache . struts . actions . dispatch action**类，这个 action 类不需要像普通 action 类那样实现 **execute()** 方法。相反，DispatchAction 类将根据传入的请求参数执行方法—**方法**。例如，如果参数是“method=chinese”，那么将执行 chinese()方法。

## 例子

action 类扩展了 DispatchAction，并包含四种方法来将 locale 设置到本地化的 Struts 会话属性中。

```java
 public class LanguageSelectAction extends DispatchAction{

	public ActionForward chinese(ActionMapping mapping,ActionForm form,
		HttpServletRequest request,HttpServletResponse response) 
	throws Exception {

		request.getSession().setAttribute(
				Globals.LOCALE_KEY, Locale.SIMPLIFIED_CHINESE);

		return mapping.findForward("success");
	}

	public ActionForward english(ActionMapping mapping,ActionForm form,
		HttpServletRequest request,HttpServletResponse response) 
	throws Exception {

		request.getSession().setAttribute(
				Globals.LOCALE_KEY, Locale.ENGLISH);

		return mapping.findForward("success");
	}

	public ActionForward german(ActionMapping mapping,ActionForm form,
		HttpServletRequest request,HttpServletResponse response) 
	throws Exception {

		request.getSession().setAttribute(
				Globals.LOCALE_KEY, Locale.GERMAN);

		return mapping.findForward("success");
	}

	public ActionForward france(ActionMapping mapping,ActionForm form,
		HttpServletRequest request,HttpServletResponse response) 
	throws Exception {

		request.getSession().setAttribute(
				Globals.LOCALE_KEY, Locale.FRANCE);

		return mapping.findForward("success");
	}

} 
```

这个 Struts html 标记将执行 chinese()方法。

```java
Chinese

```

这个 Struts html 标记将执行 english()方法。

```java
English

```

这个 Struts html 标记将执行 german()方法。

```java
German

```

这个 Struts html 标记将执行 france()方法。

```java
France

```

[struts](http://web.archive.org/web/20190223075601/http://www.mkyong.com/tag/struts/)![](img/630ad20942e42c0d9a52dedc75ea28d6.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190223075601/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4543">







