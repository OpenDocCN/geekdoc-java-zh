# jQuery–如何获取带有 CSS 类名和 id 的元素

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-how-to-get-element-with-css-class-name-and-id/>

在 jQuery 中，可以很容易地获得带有 CSS 类名和 id 的元素。

举个例子，

## 1.ID: #id

*   **$(' # idA ')**–选择 id 为' idA '的所有元素，而不考虑其标记名。
*   **$(' div # idA ')**–选择 id 为' idA '的所有 div 元素。

## 2.类别:。类名

*   **$('。classA))**–选择类名为“class a”的所有元素，而不考虑其标记名。
*   **$(' div . classA ')**–选择类名为' class a '的所有 div 元素。

## 完整示例

```java
 <html>
<head>
<title>jQuery Get element with class name and id</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

</head>

<script type="text/javascript">

$(document).ready(function(){

    var $element = $('.classABC').html();
    alert($element);

    var $element = $('#idABC').html();
    alert($element);

});

</script>
<body>

<h1>jQuery Get element with class name and id</h1>

	<div class="classABC">
		This is belong to 'div class="classABC"' 
	</div>

	<div id="idABC">
		This is belong to 'div id="idABC"'
	</div>

</body>
</html> 
```

[Try Demo](http://web.archive.org/web/20220618072947/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-get-element-class-id.html)<input type="hidden" id="mkyong-current-postId" value="4880">