# 如何用 jQuery 在运行时加载 JavaScript

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-load-javascript-at-runtime-with-jquery/>

为了提高网站性能并减少总的文件大小返回，您可以考虑加载 JavaSript(。js)文件。在 jQuery 中，可以使用 **$。getScript** 函数在运行时或按需加载 JavaScript 文件。

举个例子，

```java
 $("#load").click(function(){
	$.getScript('helloworld.js', function() {
  		$("#content").html('Javascript is loaded successful!');
	});
}); 
```

当点击一个 Id 为“load”的按钮时，会动态加载“**hello world . js**”JavaScript 文件。

## 你自己试试

在这个例子中，当你点击 load 按钮时，它将在运行时加载“ **js-example/helloworld.js** ”，其中包含一个“ **sayhello()** 函数。

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

</head>
<body>
  <h1>Load Javascript dynamically with jQuery</h1>

<div id="content"></div>
<br/>

<button id="load">Load JavaScript</button>
<button id="sayHello">Say Hello</button>

<script type="text/javascript">

$("#load").click(function(){
  $.getScript('js-example/helloworld.js', function() {
     $("#content").html('
          Javascript is loaded successful! sayHello() function is loaded!
     ');
  });
});

$("#sayHello").click(function(){
  sayHello();
});

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190223075932if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-load-javascript-at-runtime.html](http://web.archive.org/web/20190223075932if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-load-javascript-at-runtime.html)

[Try Demo](http://web.archive.org/web/20190223075932/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-load-javascript-at-runtime.html)[javascript](http://web.archive.org/web/20190223075932/http://www.mkyong.com/tag/javascript/) [jquery](http://web.archive.org/web/20190223075932/http://www.mkyong.com/tag/jquery/)![](img/a426ec502c721099e8e97bf73734d109.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190223075932/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5276">







