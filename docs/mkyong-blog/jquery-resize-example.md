# jQuery resize()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-resize-example/>

jQuery **resize()** 事件在浏览器大小改变时被触发，该事件只绑定到 **$(窗口)**。

```java
 $(window).resize(function () {
	$('#msg').text('Browser (Width : ' + $(window).width() 
	+ ' , Height :' + $(window).height() + ' )');
}); 
```

要获得浏览器的宽度和高度细节，使用 **$(window)。width()** 和 **$(窗口)。身高()**。

## 你自己试试

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	#browserInfo{
		padding:8px;
		border:1px solid blue;
		width:300px;
	}
</style>

</head>
<body>
  <h1>jQuery resize() example</h1>

  <script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>Try resize this browser</h2>

  <div id="browserInfo">
  </div>

<script type="text/javascript">

   $('#browserInfo').text('Browser (Width : '
                + $(window).width() + ' , Height :' + $(window).height() + ' )');

    $(window).resize(function () {
		$('#browserInfo').text('Browser (Width : ' + $(window).width() 
                                 + ' , Height :' + $(window).height() + ' )');
    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190213135544if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-resize-example.html](http://web.archive.org/web/20190213135544if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-resize-example.html)

[Try Demo](http://web.archive.org/web/20190213135544/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-resize-example.html)[browser event](http://web.archive.org/web/20190213135544/http://www.mkyong.com/tag/browser-event/) [jquery](http://web.archive.org/web/20190213135544/http://www.mkyong.com/tag/jquery/)![](img/ced77c363c50c86cb041a7aa63b8a737.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190213135544/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5249">







