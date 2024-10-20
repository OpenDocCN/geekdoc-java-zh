# jQuery 的页面加载效果

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/page-loading-effects-with-jquery/>

JQuery 确实是一个强大而方便的 JavaScript 库。页面或内容加载效果在 jQuery 中非常容易实现。这里有一个让你的网页内容显示页面加载淡入效果的例子。它会给你的读者留下震撼的印象。🙂

想法很简单~

## 1.创建一个内容隐藏的 div 块

这个 div 块用于实现页面加载效果中内容淡入淡出。

```java
 <div id="page_effect" style="display:none;">
<!-- this content is hide, need jQuery to fade in -->
</div> 
```

## 2.jQuery 淡入效果

当文档准备好显示时，创建一个 jQuery fadeIn 效果

```java
 $(document).ready(function(){	
	$('#page_effect').fadeIn(2000);
}); 
```

*P.S fadeIn()是 jQuery 库的内置函数。*

## 3.演示

[Try Demo](http://web.archive.org/web/20220618072947/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-page-loading-effect.html)

完成 jQuery 代码以实现淡入加载效果。

```java
 <html>
<head>
<title>JQuery Page Loading Effect</title>

<script type="text/javascript" src="jquery.js"></script>

</head>

<script type="text/javascript">

$(document).ready(function(){

	$('#page_effect').fadeIn(2000);

});

</script>
<body>

<div id="page_effect" style="display:none;">

<h1>JQuery Page Loading Effect</h1>

    This is page loading effect with JQuery 1<BR>
    This is page loading effect with JQuery 2<BR>
    This is page loading effect with JQuery 3<BR>
    This is page loading effect with JQuery 4<BR>
    This is page loading effect with JQuery 5<BR>
    This is page loading effect with JQuery 6<BR>
    This is page loading effect with JQuery 7<BR>
    This is page loading effect with JQuery 8<BR>
    This is page loading effect with JQuery 9<BR>
    This is page loading effect with JQuery 10<BR>
    This is page loading effect with JQuery 11<BR>
    This is page loading effect with JQuery 12<BR>
    This is page loading effect with JQuery 13<BR>
    This is page loading effect with JQuery 14<BR>
</div>

</body>
</html> 
```

<input type="hidden" id="mkyong-current-postId" value="432">