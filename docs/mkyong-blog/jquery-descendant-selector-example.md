# jQuery–后代选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-descendant-selector-example/>

**jQuery descender selector(X Y)**用于选择与“Y”匹配的所有元素，包括子元素、孙元素、曾孙元素、曾曾孙元素..(任何深度级别)的“X”元素。

举个例子，

*   **$(' # container div ')**–选择与< div >匹配的所有元素，这些元素是 id 为 container 的元素的后代。
*   **$(' form input ')**–选择由<输入>匹配的所有元素，这些元素是由<表单>匹配的元素的后代。

## jQuery 示例

在本例中，它使用 jQuery 后代选择器向作为

<form>元素后代的所有<input>字段添加一个“红色边框”。</form>

```java
 <html>
<head>
<title>jQuery descendant selector example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

 <style type="text/css">
  div { padding:8px 0px 8px 8px; }
 </style>

</head>

<script type="text/javascript">

$(document).ready(function(){

	$("form input").css("border", "2px solid red");

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

[http://web.archive.org/web/20220618072947if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-descendant-selector.html](http://web.archive.org/web/20220618072947if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-descendant-selector.html)

[Try Demo](http://web.archive.org/web/20220618072947/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-descendant-selector.html)<input type="hidden" id="mkyong-current-postId" value="4884">