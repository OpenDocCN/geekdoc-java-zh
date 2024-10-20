# jQuery fadeIn()、fadeOut()和 fadeTo()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-fadein-fadeout-and-fadeto-example/>

jQuery 提供了三种简便的方法来轻松创建渐变效果。

1.  fade in()–以淡入效果显示匹配的元素。
2.  fade out()–隐藏具有淡出/透明效果的匹配元素。
3.  fade to()–将匹配的元素淡化到一定的不透明度。

以上三种方法都支持持续时间作为参数:慢速、快速或精确的毫秒。如果省略此参数，则应用默认的 400 毫秒。

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	.fadeOutbox, .fadeInbox, .fadeTobox{
		float:left;
		padding:8px;
		margin:16px;
		border:1px solid red;
		width:200px;
		height:50px;
		background-color:#000000;
		color:white;
	}
	.clear{
		clear:both;
	}
</style>

</head>

<body>

<h1>jQuery fadeIn(), fadeOut() and fadeTo() example</h1>

<div class="clear">
	<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>fadeOut() example</h2>

	<div class="fadeOutbox">
		Click me - fadeOut()
	</div>

	<div class="fadeOutbox">
		Click me - fadeOut()
	</div>
</div>

<div class="clear">
	<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>fadeIn() example</h2>

	<div class="fadeInbox">
		Click me - fadeIn()
	</div>

	<div class="fadeInbox">
		Click me - fadeIn()
	</div>
</div>

<div class="clear">
	<h2>fadeTo() example</h2>

	<div class="fadeTobox">
		Click me - fadeTo()
	</div>

	<div class="fadeTobox">
		Click me - fadeTo()
	</div>
</div>

<div class="clear">
	<button id=reset>Reset</button>
</div>

<script type="text/javascript">

$(".fadeOutbox").click(function () {
   $(this).fadeOut('slow');
});

$(".fadeInbox").click(function () {
   $(this).hide().fadeIn(2000);
});

$(".fadeTobox").click(function () {
   $(this).fadeTo('fast',0.2);
});

$("#reset").click(function(){
	location.reload();
});
</script>

</body>
</html> 
```

[http://web.archive.org/web/20190216193440if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-fadeIn-faceOut-fadeTo-example.html](http://web.archive.org/web/20190216193440if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-fadeIn-faceOut-fadeTo-example.html)

[Try Demo](http://web.archive.org/web/20190216193440/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-fadeIn-faceOut-fadeTo-example.html)[jquery](http://web.archive.org/web/20190216193440/http://www.mkyong.com/tag/jquery/) [jquery effects](http://web.archive.org/web/20190216193440/http://www.mkyong.com/tag/jquery-effects/)![](img/6ea2f47cb8b8b45785aa7c051a6f0935.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190216193440/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5261">







