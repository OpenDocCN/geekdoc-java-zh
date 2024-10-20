# jQuery–子选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-child-selector-example/>

**jQuery 子选择器(X > Y)** 用于选择与“X”元素的子元素“Y”匹配的所有元素。

举个例子，

*   **$(' form>input ')**–选择所有由< input >匹配的元素，这些元素是由< form >匹配的元素的子元素，只有子元素匹配，孙子、曾孙不匹配。

## jQuery 示例

在本例中，只有“文本框 1”和“文本框 4”匹配(表单元素的子元素)，而“文本框 2”(孙元素)和“文本框 3”(曾孙元素)不匹配。

```java
 <html>
<head>
<title>jQuery child selector example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

 <style type="text/css">
  div { padding:8px 0px 8px 8px; }
 </style>

</head>

<script type="text/javascript">

$(document).ready(function(){

	$("form > input").css("border", "2px solid red");

});

</script>
<body>

<h1>jQuery child selector example</h1>

<form>

	<label>TextBox 1 (Child) : </label><input name="textbox1">

	<div class="level1">
		<label>TextBox 2 (GrandChild) : </label><input name="textbox2">
	</div>

	<div class="level1">
	   <div class="level2">
	    <label>TextBox 3 (Great GrandChild) : </label><input name="textbox3">
	   </div>
	</div>

	<label>TextBox 4 (Child) : </label><input name="textbox4">

</form>

<div> I'm form siblings #1 - DIV</div>

<p> I'm form siblings #2 - P </p>

<div> I'm form siblings #3 - DIV </div>

<p> I'm form siblings #4 - P </p>

</body>
</html> 
```

[http://web.archive.org/web/20220618072947if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-child-selector.html](http://web.archive.org/web/20220618072947if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-child-selector.html)

[Try Demo](http://web.archive.org/web/20220618072947/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-child-selector.html)<input type="hidden" id="mkyong-current-postId" value="4894">