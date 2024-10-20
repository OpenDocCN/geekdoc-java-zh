# jQuery–包含选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-contains-selector-example/>

**jQuery contains(text)选择器**用于选择包含指定文本的所有元素。

**例句**
1。$(' p:contains(paragraph 1)')–选择与包含文本“paragraph 1”的< p >匹配的所有元素。
2。$(' p:contains(mkyong)')–选择与包含文本“mkyong”的< p >匹配的所有元素。
3。$(' Li:contains(three)')–选择与包含文本“three”的< li >匹配的所有元素。

## 播放它

点击按钮来玩容器选择器。

```java
 <html>
<head>
<title>jQuery contains selector example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	#msg{
		padding:8px;
		hright:100px;
	}
</style>
</head>

<body>

<h1>jQuery contains selector example</h1>

<div id="msg"></div>

<ul>
	<li>One</li>
	<li>two</li>
	<li>three</li>
	<li>four</li>
	<li>five</li>
</ul>

<p>
	This is a paragraph 1 - google.com
</p>

<p>
	This is a paragraph 2 - mkyong.com
</p>

<br/><br/>
<button>p:contains(paragraph 1)</button>
<button>p:contains(mkyong)</button>
<button>li:contains(three)</button>

<script type="text/javascript">
    $("button").click(function () {

	  var str = $(this).text();	
	  $('p, li').css("border", "0px solid #000000");
	  $(str).css("border", "1px solid #ff0000");
	  $('#msg').html("<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>Button clicked : " + str + "</h2>");
    });

</script>

</body>
</html> 
```

[http://web.archive.org/web/20190223073152if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-contains-selector.html](http://web.archive.org/web/20190223073152if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-contains-selector.html)

[Try Demo](http://web.archive.org/web/20190223073152/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-contains-selector.html)[jquery](http://web.archive.org/web/20190223073152/http://www.mkyong.com/tag/jquery/) [jquery selector](http://web.archive.org/web/20190223073152/http://www.mkyong.com/tag/jquery-selector/)![](img/8ea0ea291f9599aee6084358efb5af63.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190223073152/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5021">







