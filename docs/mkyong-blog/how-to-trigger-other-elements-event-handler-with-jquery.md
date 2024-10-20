# 如何用 jQuery 触发其他元素事件处理程序

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-trigger-other-elements-event-handler-with-jquery/>

jQuery 附带了一个 **trigger()** 函数来执行附加到元素的事件处理程序。举个例子，

单击事件绑定到 Id 为“button1”按钮。

```java
 $("#button1").bind("click", (function () {

	alert("Button 1 is clicked!");

})); 
```

单击事件绑定到 Id 为“button2”按钮。以及执行 button1 click 事件处理程序的触发器。

```java
 $("#button2").bind("click", (function () {

	alert("Button 2 is clicked!");

	$("#button1").trigger("click");

})); 
```

当点击按钮 2 时，警告消息“**按钮 2 被点击！**”是提示，随后是按钮 1 警告消息“**按钮 1 被点击！**’。

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

</head>

<body>

<h1>jQuery trigger() example</h1>

<script type="text/javascript">

  $(document).ready(function(){

    $("#button1").bind("click", (function () {

	alert("Button 1 is clicked!");

    }));

    $("#button2").bind("click", (function () {

	alert("Button 2 is clicked!");

	$("#button1").trigger("click");

    }));

  });
</script>
</head><body>

<input type='button' value='Button 1' id='button1'>
<input type='button' value='Button 2' id='button2'>

</body>
</html> 
```

[http://web.archive.org/web/20190309091056if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-trigger-example.html](http://web.archive.org/web/20190309091056if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-trigger-example.html)

[Try Demo](http://web.archive.org/web/20190309091056/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-trigger-example.html)[jquery](http://web.archive.org/web/20190309091056/http://www.mkyong.com/tag/jquery/) [jquery event handler](http://web.archive.org/web/20190309091056/http://www.mkyong.com/tag/jquery-event-handler/)![](img/cc2a1c539f1a54cf4b2b9500a1520676.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190309091056/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5180">







