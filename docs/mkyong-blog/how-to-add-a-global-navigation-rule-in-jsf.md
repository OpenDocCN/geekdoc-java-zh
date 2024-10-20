# 如何在 JSF 添加全球导航规则？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/how-to-add-a-global-navigation-rule-in-jsf/>

## 问题

全局导航规则是适用于所有页面的规则。例如，应用允许每个页面访问“**注销**”导航规则。有办法在 JSF 配置吗？

 ## 解决办法

在 JSF，导航规则支持通配符，您可以使用“ ***** ”来指定适用于所有页面的导航规则。

```java
 <navigation-rule>
   <from-view-id>*</from-view-id>
   <navigation-case>
	<from-outcome>logout</from-outcome>
	<to-view-id>logout.xhtml</to-view-id>
   </navigation-case>
</navigation-rule> 
```

在本例中，您的 web 应用程序中的所有页面都能够访问“ **logout** ”导航。

看另一个例子，

```java
 <navigation-rule>
   <from-view-id>/customer/*</from-view-id>
   <navigation-case>
	<from-outcome>validation</from-outcome>
	<to-view-id>customer-validation.xhtml</to-view-id>
   </navigation-case>
</navigation-rule> 
```

这条规则适用于所有以前缀 **/customer/** 开头的页面。

**Note**
The “*****” wildcard functionality is limited in JSF, because only one “*****” is allowed, and it must be at the end of the “**form-view-id**” string. For example,

它工作了。

```java
 <from-view-id>/customer/*</from-view-id> 
```

永远比不上…

```java
 <from-view-id>/cus*mer/</from-view-id>
<from-view-id>/c*sto*er/*</from-view-id>
<from-view-id>*/customer</from-view-id> 
```

[jsf](http://web.archive.org/web/20190223132221/http://www.mkyong.com/tag/jsf/) [jsf2](http://web.archive.org/web/20190223132221/http://www.mkyong.com/tag/jsf2/) [navigation rule](http://web.archive.org/web/20190223132221/http://www.mkyong.com/tag/navigation-rule/)![](img/020b9171b3f48ca36c5bb74ed8445b75.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190223132221/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="7107">







