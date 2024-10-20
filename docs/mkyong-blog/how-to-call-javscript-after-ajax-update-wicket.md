# AJAX 更新后如何调用 Javscript，Wicket

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/how-to-call-javscript-after-ajax-update-wicket/>

## 问题

根据这个[这个检票口的文章](http://web.archive.org/web/20190112014028/http://cwiki.apache.org/WICKET/howto-do-javscript-call-after-each-wicket-ajax-update.html)。如果我们想在每次 Ajax 更新后运行某段 Javascript 代码，我们可以像这样使用“`wicketGlobalPostCallHandler`”:

```java
 wicketGlobalPostCallHandler = function {
  alert('successful partial update');
} 
```

然而，这个解决方案对我来说并不奏效。

 ## 解决办法

或者，您可以使用“`ajaxRequestTarget`”来附加 Javascrip 代码:

```java
 ajaxRequestTarget.appendJavascript("alert('hello');"); 
```

[ajax](http://web.archive.org/web/20190112014028/http://www.mkyong.com/tag/ajax/) [javascript](http://web.archive.org/web/20190112014028/http://www.mkyong.com/tag/javascript/) [wicket](http://web.archive.org/web/20190112014028/http://www.mkyong.com/tag/wicket/)![](img/df2456be1f3b9440379476aced587531.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190112014028/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="1276">







