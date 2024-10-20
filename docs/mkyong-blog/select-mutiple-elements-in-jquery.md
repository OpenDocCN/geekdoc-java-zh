# 在 jquery 中选择多个元素

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/select-mutiple-elements-in-jquery/>

在 jQuery 中，可以选择多个元素，用逗号“，”符号分隔。

举个例子，

```java
 $(.class1, .class2, #id1) 
```

在上面的例子中，选择类名为“class1”和“class2”，id 为“id1”的所有元素。

## jQuery 示例

在本例中，类名为“ **p1** ”和“ **p2** ”、id 为“ **div3** ”的元素将被动态添加红色边框。

```java
 <html>
<head>
<title>Select mutiple elements example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

</head>
<script type="text/javascript">

$(document).ready(function(){

	$(".p1, .p3, #div3").css("border", "2px solid red");

});

</script>
<body>

<h1>Select mutiple elements example</h1>

<p class="p1">P1</p>
<p class="p2">P2</p>
<p class="p3">P3</p>
<p class="p4">P4</p>

<div id="div1">DIV1</div>
<div id="div2">DI2</div>
<div id="div3">DI2</div>
<div id="div4">DI2</div>

</body>
</html> 
```

[http://web.archive.org/web/20190310100517if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-select-mutiple-elements.html](http://web.archive.org/web/20190310100517if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-select-mutiple-elements.html)

[Try Demo](http://web.archive.org/web/20190310100517/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-select-mutiple-elements.html)[jquery](http://web.archive.org/web/20190310100517/http://www.mkyong.com/tag/jquery/) [jquery selector](http://web.archive.org/web/20190310100517/http://www.mkyong.com/tag/jquery-selector/)![](img/1d85516341199a975a3ace39c7fc83a8.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190310100517/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4944">







