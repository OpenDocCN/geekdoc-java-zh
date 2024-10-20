# jQuery after()和 insertAfter()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-after-and-insertafter-example/>

jQuery **after()** 和 **insertAfter()** 方法都在执行相同的任务，在匹配的元素后添加文本或 html 内容。主要的区别在于语法。

举个例子，

```java
 <div class="greyBox">I'm a ipman</div>
<div class="greyBox">I'm a ipman 2</div> 
```

## 1.$('选择器')。后('新内容')；

```java
 $('.greyBox').after("<div class='redBox'>Iron man</div>"); 
```

## 2.$('新内容')。insertAfter(“选择器”)；

```java
 $("<div class='redBox'>Iron man</div>").insertAfter('.greyBox'); 
```

## 结果

以上两种方法做的是同样的任务，但是语法不同，在()或 **insertAfter()** 之后的新内容将变成

```java
 <div class="greyBox">
   I'm a ipman
</div>
<div class='redBox'>Iron man</div>

<div class="greyBox">
   I'm a ipman 2
</div>
<div class='redBox'>Iron man</div> 
```

## 你自己试试

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	.greyBox{
		padding:8px;
		background: grey;
		margin-bottom:8px;
		width:300px;
		height:100px;
	}
	.redBox{
		padding:8px;
		background: red;
		margin-bottom:8px;
		width:200px;
		height:50px;
	}
</style>

</head>
<body>
  <h1>jQuery after() and insertAfter() example</h1>

  <div class="greyBox">Ip man</div>

  <div class="greyBox">Ip man 2</div>

  <p>
  <button id="after">after()</button>
  <button id="insertAfter">insertAfter()</button>
  <button id="reset">reset</button>
  </p>

<script type="text/javascript">

    $("#after").click(function () {

	  $('.greyBox').after("<div class='redBox'>Iron man</div>");

    });

	$("#insertAfter").click(function () {

	  $("<div class='redBox'>Iron man</div>").insertAfter('.greyBox');

    });

	$("#reset").click(function () {
	  location.reload();
    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-after-insertAfter-example.html](http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-after-insertAfter-example.html)

[Try Demo](http://web.archive.org/web/20220826132030/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-after-insertAfter-example.html)<input type="hidden" id="mkyong-current-postId" value="5150">