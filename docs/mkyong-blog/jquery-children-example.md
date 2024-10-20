# jQuery children()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-children-example/>

在 jQuery 中， **children()** 用于查找匹配元素的子元素，只是**向下移动了一层**。

例如，深度为三级的 div 元素包含一个类名“child”和“orphan”。

```java
 <div class="A1">
	<div class="child">A1-1</div>
	<div class="child">A1-2</div>
	<div class="orphan">A1-3</div>
	<div class="child">A1-4</div>

	<div class="A2">
		<div class="child">A2-1</div>
		<div class="child">A2-2</div>

		<div class="A3">
			<div class="child">A3-1</div>
			<div class="child">A3-2</div>
		</div>
	</div>

</div> 
```

## 1.$('.A1 ')。儿童()

```java
 $('.A1').children().css('background','red'); 
```

匹配元素包含一个类名为“ **A1** ”，以及所有的**A1**“**单级子级**”。

A2 和 A3 孩子将被忽略。

 ## 2.$('.A1 ')。孩子(’。孩子’)

```java
 $('.A1').children('.child').css('background','red'); 
```

匹配包含类名“ **A1** ”的元素，并搜索包含类名“child”的“ **A1** ”子元素。在本例中，**除了类名为“孤儿”的孩子外，所有 A1 的“单级孩子”都将被选择**。

 ## jQuery children()示例

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

<h1>jQuery children() example</h1>

<script type="text/javascript">

  $(document).ready(function(){

    $("#buttonChildren1").click(function () {

	$('div').css('background','white');

	$('.A1').children().css('background','red');

    });

    $("#buttonChildren2").click(function () {

	$('div').css('background','white');

	$('.A1').children('.child').css('background','red');

    });

  });
</script>
</head><body>

<div class="A1">
	<div class="child">A1-1</div>
	<div class="child">A1-2</div>
	<div class="orphan">A1-3</div>
	<div class="child">A1-4</div>

	<div class="A2">
		<div class="child">A2-1</div>
		<div class="child">A2-2</div>

		<div class="A3">
			<div class="child">A3-1</div>
			<div class="child">A3-2</div>
		</div>
	</div>

</div>

<br/>
<br/>
<br/>

<input type='button' value='.A1 children()' id='buttonChildren1'>
<input type='button' value='.A1 children(child)' id='buttonChildren2'>

</body>
</html> 
```

[http://web.archive.org/web/20190215032140if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-children-example.html](http://web.archive.org/web/20190215032140if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-children-example.html)

[Try Demo](http://web.archive.org/web/20190215032140/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-children-example.html)[jquery](http://web.archive.org/web/20190215032140/http://www.mkyong.com/tag/jquery/) [jquery traversing](http://web.archive.org/web/20190215032140/http://www.mkyong.com/tag/jquery-traversing/)







