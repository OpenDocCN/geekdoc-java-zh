# jQuery mouseup()和 mousedown()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-mouseup-and-mousedown-example/>

jQuery **mouseup()** 和 **mousedown()** 事件都是不言自明的，用于验证鼠标按钮被按下或释放。

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	#mouseup, #mousedown{
		float:left;
		padding:8px;
		margin:8px;
		border:1px solid red;
		width:200px;
		height:150px;
		background-color:#999999;
	}

</style>

</head>

<body>

<h1>jQuery mouseup() and mousedown() examples</h1>

<div id="mouseup">
	<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>mouseup</h2>
	 Fire when mouse over this element and released the mouse button.
</div>

<div id="mousedown">
	<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>mousedown</h2>
	Fire when mouse over this element and pressed on the mouse button.
</div>

<script type="text/javascript">

$('#mouseup').mouseup(function(){
	$('#mouseup').slideUp();
});

$('#mousedown').mousedown(function(){
	$('#mousedown').fadeOut();
});

</script>

</body>
</html> 
```

[http://web.archive.org/web/20190214232535if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mouseup-mousedown-example.html](http://web.archive.org/web/20190214232535if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mouseup-mousedown-example.html)

[Try Demo](http://web.archive.org/web/20190214232535/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mouseup-mousedown-example.html)[jquery](http://web.archive.org/web/20190214232535/http://www.mkyong.com/tag/jquery/) [mouse event](http://web.archive.org/web/20190214232535/http://www.mkyong.com/tag/mouse-event/)![](img/1439b36d88a9cea59c0bfc3c0a7ad5a4.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214232535/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5228">







