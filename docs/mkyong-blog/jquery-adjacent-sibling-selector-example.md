# jQuery–相邻兄弟选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-adjacent-sibling-selector-example/>

**jQuery 相邻兄弟选择器(X + Y)** 用于选择与“X”元素的兄弟元素“Y”匹配的紧接着的或下一个元素。

举个例子，

```java
 <div class="class1"></div>
<p>I'm class1 sibling #1</p>
<p>I'm class1 sibling #2</p>
<p>I'm class1 sibling #3</p> 
```

和

是兄弟姐妹关系。“ **$(.class1 + p)** ”语句将只选择值为“ **I'm class1 sibling #1** ”的< p >元素，其余的< p >元素将被忽略。

## jQuery 示例

在这个例子中，只有值为"**I ' m form siblings # 1–DIV**"的

标签将被匹配，因为它是表单>元素<的紧接其后的或下一个兄弟。

```java
 <html>
<head>
<title>jQuery adjacent sibling selector example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

 <style type="text/css">
  div { padding:8px 0px 8px 8px; }
 </style>

</head>

<script type="text/javascript">

$(document).ready(function(){

	$("form + div").css("border", "2px solid red");

});

</script>
<body>

<h1>jQuery adjacent sibling selector example</h1>

<form>

	<label>TextBox 1 (Child) : </label><input name="textbox1">

	<div class="level1">
		<label>TextBox 2 (GrandChild) : </label><input name="textbox2">
	</div>

	<div class="level1">
	   <div class="level2">
	    <label>TextBox 3 (Great GrandChild) : </label><input name="textbox3">
	   </div>
	</div>

	<label>TextBox 4 (Child) : </label><input name="textbox4">

</form>

<div> I'm form siblings #1 - DIV</div>

<p> I'm form siblings #2 - P </p>

<div> I'm form siblings #3 - DIV </div>

<p> I'm form siblings #4 - P </p>

</body>
</html> 
```

[http://web.archive.org/web/20190224162730if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-adjacent-sibling-selector.html](http://web.archive.org/web/20190224162730if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-adjacent-sibling-selector.html)

[Try Demo](http://web.archive.org/web/20190224162730/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-adjacent-sibling-selector.html)[jquery](http://web.archive.org/web/20190224162730/http://www.mkyong.com/tag/jquery/) [jquery selector](http://web.archive.org/web/20190224162730/http://www.mkyong.com/tag/jquery-selector/)![](img/36924840a51e59ac42e992f8992348e2.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224162730/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4914">







