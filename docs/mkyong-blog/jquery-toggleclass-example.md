# jQuery toggleClass 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-toggleclass-example/>

**toggleClass()** 方法意味着如果匹配的元素没有类名，那么添加它；如果匹配的元素已经有了类名，那么删除它。

让我们看一个例子，一个简单的“p”标签。

```java
 <p>This is paragraph</p> 
```

调用 **$('p ')。toggleClass('highlight')** ，它会给' p '标签添加一个高亮类。

```java
 <p class="highlight">This is paragraph</p> 
```

调用 **$('p ')。toggleClass('highlight')** 再次，它将从' p '标记中删除高亮显示的类。

```java
 <p>This is paragraph</p> 
```

toggleClass()也等效于下面的 jQuery 代码。

```java
 if ($('p').is('.highlight')) {
    $('p').removeClass('highlight');
}
else{
    $('p').addClass('highlight');
} 
```

## jQuery toggleClass 示例

```java
 <html>
<head>
<style type="text/css">
  .highlight { 
  	background:blue;
  }
 </style>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>
</head>
<body>
  <h1>jQuery toggleClass example</h1>

  <p>This is paragraph1</p>
  <p id="para2">This is paragraph2 (toggleClass effect)</p>
  <p>This is paragraph3</p>
  <p id="para4">This is paragraph4 (add/remove effect)</p>

  <button id="toggleClass">toggle class</button>
  <button id="addremoveClass">Add/Remove class</button>

<script type="text/javascript">

    $("#toggleClass").click(function () {

	  $('#para2').toggleClass('highlight');

    });

    $("#addremoveClass").click(function () {

	  if ($('#para4').is('.highlight')) {

	      $('#para4').removeClass('highlight');

	  }
	  else{

	      $('#para4').addClass('highlight');

	  }

    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20220826132031if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-toggleClass-example.html](http://web.archive.org/web/20220826132031if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-toggleClass-example.html)

[Try Demo](http://web.archive.org/web/20220826132031/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-toggleClass-example.html)<input type="hidden" id="mkyong-current-postId" value="5116">