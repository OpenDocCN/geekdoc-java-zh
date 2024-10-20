# jQuery prepend()和 prependTo()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-prepend-and-prependto-example/>

jQuery **prepend()** 和 **prependTo()** 方法都在执行相同的任务，在匹配元素的内容之前添加一个文本或 html 内容。主要的区别在于语法。

举个例子，

```java
 <div class="box">I'm a big box</div>
<div class="box">I'm a big box 2</div> 
```

## 1.$('选择器')。前置(“新文本”)；

```java
 $('.box').prepend("<div class='newbox'>I'm new box by prepend</div>"); 
```

## 2.$('新文本')。前置 to(' selector ')；

```java
 $("<div class='newbox'>I'm new box by prependTo</div>").prependTo('.box'); 
```

## 结果

上面两种方法做的是同样的任务，但是语法不同，在 **prepend()** 或 **prependTo()** 之后的新内容将变成
这是一个非常通用的工具。对于任何你想要一个滑动条的网站来说，这就是方法。如果你看看任何一个大网站，比如像 www.o2.co.uk/这样的网站或者一个电影网站，你很可能会看到一个正在使用的网站。这是一个很好的方式，让图像旋转看起来很聪明，并抓住观众的眼睛。它给你的网站增加了一个不同的维度，这将使它真正脱颖而出。

```java
 <div class="box">
   <div class='newbox'>I'm new box by prepend</div>
   I'm a big box
</div>

<div class="box">
   <div class='newbox'>I'm new box by prepend</div>
   I'm a big box 2
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
  <h1>jQuery prepend() and prependTo example</h1>

  <div class="box">I'm a big box</div>

  <div class="box">I'm a big box 2</div>

  <p>
  <button id="prepend">prepend()</button>
  <button id="prependTo">prependTo()</button>
  <button id="reset">reset</button>
  </p>

<script type="text/javascript">

    $("#prepend").click(function () {

	  $('.box').prepend("<div class='newbox'>I'm new box by prepend</div>");

    });

    $("#prependTo").click(function () {

	  $("<div class='newbox'>I'm new box by prependTo</div>").prependTo('.box');

    });

    $("#reset").click(function () {
	  location.reload();
    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-prepend-prependTo-example.html](http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-prepend-prependTo-example.html)

[Try Demo](http://web.archive.org/web/20220826132030/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-prepend-prependTo-example.html)<input type="hidden" id="mkyong-current-postId" value="5135">