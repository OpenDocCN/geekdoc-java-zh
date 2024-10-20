# jQuery——如何获得标记名

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-how-to-get-the-tag-name/>

要获得元素标记名，可以使用**标记名**函数。有两种方法可以使用它:

## 1) .get(0).tagName

选择一个类名为“classTag1”的元素，并使用**。获取(0)。标记名**函数显示其标记名。

```java
 $('.classTag1').get(0).tagName; 
```

## 页:1。[0].tagName

2.选择一个类名为“classTag1”的元素，并使用**。[0].标记名**函数显示其标记名。

```java
 $('.classTag1')[0].tagName; 
```

## 例子

```java
 <html>
<head>
<title>jQuery Get Tag Name</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

</head>

<script type="text/javascript">

$(document).ready(function(){

    var $tag = $('p')[0].tagName; //'P'
    alert($tag);

    var $tag = $('.classTag1')[0].tagName; //'DIV'
    alert($tag);

    var $tag = $('#idTag1')[0].tagName; //'DIV'
    alert($tag);

    var $tag = $('p').get(0).tagName; //'P'
    alert($tag);

    var $tag = $('.classTag1').get(0).tagName; //'DIV'
    alert($tag);

    var $tag = $('#idTag1').get(0).tagName; //'DIV'
    alert($tag);	

});

</script>
<body>

<h1>jQuery Get Tag Name</h1>

    <p>
    	This is paragrah 1
    </p>

	<div class="classTag1">
		This is class='classTag1'
	</div>

	<div id="idTag1">
		This is id='idTag1'
	</div>

</body>
</html> 
```

[Try Demo](http://web.archive.org/web/20220618072948/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-get-tag-name.html)<input type="hidden" id="mkyong-current-postId" value="4872">