# jQuery show()和 hide()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-show-and-hide-example/>

jQuery show()和 hide()是最常用的效果。

1.  show()–显示匹配的元素。
2.  Hide()–隐藏匹配的元素。

这两种方法都支持将持续时间作为参数:慢、快或精确的毫秒。如果省略此参数，则应用默认的 400 毫秒。

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	p{
		padding:8px;
		margin:16px;
		border:1px solid blue;
		width:250px;
		height:50px;
		background-color:#999999;
		color:white;
	}
</style>

</head>

<body>

<h1>jQuery show() and hide() example</h1>

<p>Hello, this show() and hide() example</p>

<button id=show>show()</button>
<button id=hide>hide()</button>
<button id=reset>Reset</button>

<script type="text/javascript">

$("#show").click(function () {
   $("p").show('fast');
});

$("#hide").click(function () {
   $("p").hide(1000);
});

$("#reset").click(function(){
	location.reload();
});
</script>

</body>
</html> 
```

[http://web.archive.org/web/20190608151308if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-show-hide-example.html](http://web.archive.org/web/20190608151308if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-show-hide-example.html)

[Try Demo](http://web.archive.org/web/20190608151308/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-show-hide-example.html)[jquery](http://web.archive.org/web/20190608151308/https://www.mkyong.com/tag/jquery/) [jquery effects](http://web.archive.org/web/20190608151308/https://www.mkyong.com/tag/jquery-effects/)![](img/85968dac5f7d1565a910744365f02267.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190608151308/https://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5271">







