# jQuery click()和 dblclick()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-click-and-dblclick-example/>

jQuery **click()** 和 **dblclick()** 事件是最常用的鼠标事件:

1.  click()–当鼠标单击匹配的元素时触发。
2.  dblclick()–当鼠标双击匹配的元素时触发。

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	#singleClick, #doubleClick{
		float:left;
		padding:8px;
		margin:16px;
		border:1px solid blue;
		width:150px;
		height:150px;
		background-color:#999966;
	}

</style>

</head>

<body>

<h1>jQuery click() and dblclick() example</h1>

<div id="singleClick">
	Single Click Me
</div>

<div id="doubleClick">
	Double Clicks Me
</div>

<script type="text/javascript">

$('#singleClick').click(function(){
	$('#singleClick').slideUp();
});

$('#doubleClick').dblclick(function(){
	$('#doubleClick').fadeOut();
});

</script>

</body>
</html> 
```

[http://web.archive.org/web/20190304032700if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-click-dblclick-example.html](http://web.archive.org/web/20190304032700if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-click-dblclick-example.html)

[Try Demo](http://web.archive.org/web/20190304032700/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-click-dblclick-example.html)[jquery](http://web.archive.org/web/20190304032700/http://www.mkyong.com/tag/jquery/) [mouse movement](http://web.archive.org/web/20190304032700/http://www.mkyong.com/tag/mouse-movement/)![](img/59617c15a97950c0eb55b624775ee0cf.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304032700/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5224">







