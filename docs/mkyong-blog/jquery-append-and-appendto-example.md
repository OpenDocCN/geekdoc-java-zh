# jQuery append()和 appendTo()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-append-and-appendto-example/>

jQuery **append()** 和 **appendTo()** 方法都在执行相同的任务，在匹配元素的内容之后添加一个文本或 html 内容。主要的区别在于语法。

举个例子，

```java
 <div class="box">I'm a big box</div>
<div class="box">I'm a big box 2</div> 
```

## 1.$('选择器')。append(“新文本”)；

```java
 $('.box').append("<div class='newbox'>I'm new box by prepend</div>"); 
```

## 2.$('新文本')。appendTo(' selector ')；

```java
 $("<div class='newbox'>I'm new box by appendTo</div>").appendTo('.box'); 
```

## 结果

以上两种方法做的是同样的任务，但是语法不同， **append()** 或 **appendTo()** 之后的新内容将变成

```java
 <div class="box">
   I'm a big box
   <div class='newbox'>I'm new box by prepend</div>
</div>

<div class="box">
   I'm a big box 2
   <div class='newbox'>I'm new box by prepend</div>
</div> 
```

## 你自己试试

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	.box{
		padding:8px;
		border:1px solid blue;
		margin-bottom:8px;
		width:300px;
		height:100px;
	}
	.newbox{
		padding:8px;
		border:1px solid red;
		margin-bottom:8px;
		width:200px;
		height:50px;
	}
</style>

</head>
<body>
  <h1>jQuery append() and appendTo example</h1>

  <div class="box">I'm a big box</div>

  <div class="box">I'm a big box 2</div>

  <p>
  <button id="append">append()</button>
  <button id="appendTo">appendTo()</button>
  <button id="reset">reset</button>
  </p>

<script type="text/javascript">

    $("#append").click(function () {

	  $('.box').append("<div class='newbox'>I'm new box by append</div>");

    });

	$("#appendTo").click(function () {

	  $("<div class='newbox'>I'm new box by appendTo</div>").appendTo('.box');

    });

	$("#reset").click(function () {
	  location.reload();
    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-append-appendTo-example.html](http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-append-appendTo-example.html)

[Try Demo](http://web.archive.org/web/20220826132030/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-append-appendTo-example.html)<input type="hidden" id="mkyong-current-postId" value="5141">