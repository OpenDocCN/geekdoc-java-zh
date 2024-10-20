# jQuery 中 find()和 children()的区别

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/difference-between-find-and-children-in-jquery/>

**[find()](http://web.archive.org/web/20221023054404/http://www.mkyong.com/jquery/jquery-find-example/)** 和 **[children()](http://web.archive.org/web/20221023054404/http://www.mkyong.com/jquery/jquery-children-example/)** 两种方法都用来过滤匹配元素的子元素，只不过前者是向下遍历任意一级，后者是向下遍历单级。

太简单了

1.  **find()**–搜索匹配元素的子元素、孙元素、曾孙元素…任何下一层。
2.  **children()**–仅搜索匹配元素的子元素(向下一级)。

参见下面的例子来理解 find()和 children() 之间的**区别。**

## jQuery find() vs children()示例

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	div{
		padding:8px;
		border:1px solid;
	}
</style>

</head>

<body>

<h1>jQuery find() vs children() example</h1>

<script type="text/javascript">

  $(document).ready(function(){

	$("#testChildren").click(function () {

		$('div').css('background','white');

		$('.B1').children('.child').css('background','red');

        });

	$("#testFind").click(function () {

		$('div').css('background','white');

		$('.B1').find('.child').css('background','red');

        });

  });
</script>
</head><body>

<div class="B1">
	<div class="child">B1-1</div>
	<div class="child">B1-2</div>
	<div class="orphan">B1-3 - Orphan</div>
	<div class="child">B1-4</div>

	<div class="B2">
		<div class="child">B2-1</div>
		<div class="child">B2-2</div>
		<div class="orphan">B2-2 - Orphan</div>

		<div class="B3">
			<div class="child">B3-1</div>
			<div class="orphan">B3-2 - Orphan</div>
			<div class="child">B3-3</div>
		</div>
	</div>

</div>

<br/>
<br/>
<br/>

<input type='button' value='.B1 children(child)' id='testChildren'>
<input type='button' value='.B1 find(child)' id='testFind'>

</body>
</html> 
```

[http://web.archive.org/web/20221023054404if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-find-children-example.html](http://web.archive.org/web/20221023054404if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-find-children-example.html)

[Try Demo](http://web.archive.org/web/20221023054404/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-find-children-example.html)<input type="hidden" id="mkyong-current-postId" value="5087">