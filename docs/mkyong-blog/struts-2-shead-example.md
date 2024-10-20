# Struts 2 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-shead-example/>

**< s:head >** 标签用于输出编码、CSS 或 JavaScript 文件等 HTML 头信息。请参见以下片段:

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<html>
<head>
<s:head />
</head>
<body>
.. 
```

假设您使用默认的 xhtml 主题，它将根据" **template\xhtml\head.ftl** "文件呈现输出

```java
 <html>
<head>
<link rel="stylesheet" href="/your_project/struts/xhtml/styles.css" type="text/css"/> 
<script src="/your_project/struts/utils.js" type="text/javascript"></script> 
</head>
<body>
.. 
```

要包含新的 js 或 css 文件，只需将其添加到“ **template\xhtml\head.ftl** 模板文件中，并通过 **< s:head >** 标签输出即可。

实际上，这个 **< s:head >** 标签并不需要放在 HTML < head >标签的上面并变形，例如

```java
 <head>
<s:head />
</head> 
```

你可以把它放在任何地方，它只是输出 CSS 和 js 文件的路径(默认在 xhtml 主题中)。

```java
 <head>
</head>
<body>
<s:head />
... 
```

**Good Practice**

为了提高网站的性能，最好的做法是将 CSS 文件放在页面的顶部；而 js 文件在页面底部。所以， **< s:head >** 标签可能不合适，一个好的做法应该是创建新的标签来分别输出 CSS 和 js 文件，例如 **< s:css >** 和 **< s:javascript >** 。

## 参考

1.  [支柱 2 < s:头部>示例](http://web.archive.org/web/20190222111847/http://struts.apache.org/2.x/docs/head.html)

[struts2](http://web.archive.org/web/20190222111847/http://www.mkyong.com/tag/struts2/)![](img/7b7df93610fd9a6a14c11e6d22c58014.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190222111847/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5992">







