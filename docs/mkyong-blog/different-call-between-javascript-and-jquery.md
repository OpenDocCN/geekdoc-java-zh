# JavaScript 和 JQuery 中函数调用的不同

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/different-call-between-javascript-and-jquery/>

这是一个简单的例子，演示了如何在 JavaScript 和 jQuery 中调用一个函数来创建页面加载后的动态内容。

## 1.Java Script 语言

为了在页面加载后调用函数，JavaScript 使用“ **window.onload** ”和“ **innerHTML** ”来动态创建内容。

```java
 window.onload = function(){
 document.getElementById('msgid3')
         .innerHTML = "This is Hello World by JavaScript";
} 
```

 ## 2.jQuery

为了在页面加载后调用函数，jQuery 使用了" **$(document)。准备好**，和 **html()** 来动态创建内容。

```java
 $(document).ready(function(){
 $("#msgid1").html("This is Hello World by JQuery 1<BR>");
}); 
```

 ## 例子

```java
 <html>
<head>
<title>jQuery Hello World</title>
</head>
<script type="text/javascript" src="jquery-1.2.6.min.js"></script>
<body>

<script type="text/javascript">

$(document).ready(function(){
 $("#msgid1").html("This is Hello World by JQuery 1<BR>");
});

$(function(){
 $("#msgid2").html("This is Hello World by JQuery 2<BR>");
});

window.onload = function(){
 document.getElementById('msgid3').innerHTML = "This is Hello World by JavaScript";
}

</script>

This is Hello World by HTML

<div id="msgid1">
</div>

<div id="msgid2">
</div>

<div id="msgid3">
</div>

</body>
</html> 
```

![jquery-function-call-javascript](img/5ab2625f42e7ba3071d22c5c226e75ae.png "jquery-function-call-javascript")[function](http://web.archive.org/web/20190302180930/http://www.mkyong.com/tag/function/) [javascript](http://web.archive.org/web/20190302180930/http://www.mkyong.com/tag/javascript/)







