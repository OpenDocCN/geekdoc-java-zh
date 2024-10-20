# jQuery before()和 insertBefore()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-before-and-insertbefore-example/>

jQuery **before()** 和 **insertBefore()** 方法都在执行相同的任务，在匹配的元素之前添加文本或 html 内容。主要的区别在于语法。

举个例子，

```java
 <div class="prettyBox">I'm a ipman</div>
<div class="prettyBox">I'm a ipman 2</div> 
```

## 1.$('选择器')。之前(“新内容”)；

```java
 $('.prettyBox').before("<div class='newPrettybox'>Iron man</div>"); 
```

## 2.$('新内容')。insertBefore(“选择器”)；

```java
 $("<div class='newPrettybox'>Iron man</div>").insertBefore('.prettyBox'); 
```

## 结果

以上两种方法做的是同样的任务，但是语法不同，新的内容在**之后()**或者 **insertBefore()** 之后会变成

```java
 <div class='newPrettybox'>Iron man</div>
<div class="prettyBox">
   I'm a ipman
</div>

<div class='newPrettybox'>Iron man</div>
<div class="prettyBox">
   I'm a ipman 2
</div> 
```

## 你自己试试

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	.prettyBox{
		padding:8px;
		background: grey;
		margin-bottom:8px;
		width:300px;
		height:100px;
	}
	.newPrettybox{
		padding:8px;
		background: red;
		margin-bottom:8px;
		width:200px;
		height:50px;
	}
</style>

</head>
<body>
  <h1>jQuery before() and insertBefore() example</h1>

  <div class="prettyBox">Ip man</div>

  <div class="prettyBox">Ip man 2</div>

  <p>
  <button id="before">before()</button>
  <button id="insertBefore">insertBefore()</button>
  <button id="reset">reset</button>
  </p>

<script type="text/javascript">

    $("#before").click(function () {

	  $('.prettyBox').before("<div class='newPrettybox'>Iron man</div>");

    });

	$("#insertBefore").click(function () {

	  $("<div class='newPrettybox'>Iron man</div>").insertBefore('.prettyBox');

    });

	$("#reset").click(function () {
	  location.reload();
    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-before-insertBefore-example.html](http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-before-insertBefore-example.html)

[Try Demo](http://web.archive.org/web/20220826132030/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-before-insertBefore-example.html)<input type="hidden" id="mkyong-current-postId" value="5145">