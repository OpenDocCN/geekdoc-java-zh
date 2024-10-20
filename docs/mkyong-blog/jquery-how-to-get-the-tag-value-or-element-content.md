# jQuery——如何获取标签值或元素内容

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-how-to-get-the-tag-value-or-element-content/>

在 jQuery 中，您可以从选定的标记中获取标记值或元素内容。

举个例子，

1.选择标记名“p”并显示其标记值。

```java
 $('p').html(); 
```

2.选择类名为“class1”的元素并显示其值。不管标签名称如何。

```java
 $('.class1').html(); 
```

3.选择 id 为“id1”的元素并显示其值。不管标签名称如何。

```java
 $('#id1').html(); 
```

## jQuery 示例

```java
 <html>
<head>
<title>jQuery Get Tag Value</title>

<script type="text/javascript" src="jquery-1.3.2.js"></script>

</head>

<script type="text/javascript">

$(document).ready(function(){
    var $temp = $('p').html();
    alert($temp);

    var $temp = $('.class1').html();
    alert($temp);

    var $temp = $('#id1').html();
    alert($temp);

});

</script>
<body>

<h1>jQuery Get Tag Value</h1>

    <p>
    	This is paragrah 1
    </p>

	<div class="class1">
		This is class='class1'
	</div>

	<div id="id1">
		This is id='id1'
	</div>

</body>
</html> 
```

[Try Demo](http://web.archive.org/web/20220618072947/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-get-tag-value.html)<input type="hidden" id="mkyong-current-postId" value="4866">