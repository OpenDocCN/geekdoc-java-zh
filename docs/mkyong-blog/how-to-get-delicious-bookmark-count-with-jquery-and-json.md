# 如何用 jQuery 和 JSON 获得好吃的书签计数

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-get-delicious-bookmark-count-with-jquery-and-json/>

最好的书签网站 Delicious ，提供了许多 API 让开发者处理书签的数据。下面是一个使用 jQuery 检索给定 URL 的书签总数的例子。

## 美味 API

要获得书签总数，请使用以下命令

```java
 http://feeds.delicious.com/v2/json/urlinfo/data?url=xxx.com&callback=? 
```

## jQuery Ajax

jQuery 附带了一个简单而强大的**。ajax()** 或简写**。getJSON()** 按需获取远程数据。

##### 1.jQuery。ajax()示例

使用 jQuery **。ajax()** 从 Delicious 获取 json 数据，并显示书签计数的总数。

```java
 $.ajax({ 
 type: "GET",
 dataType: "json",
 url: "http://feeds.delicious.com/v2/json/urlinfo/data?url="+url+"&amp;callback=?",
 success: function(data){			
	var count = 0;
	if (data.length > 0) {
		count = data[0].total_posts;
	}
	$("#delicious_result").text(count + ' Saved');			
 }
 }); 
```

##### 2.jQuery。getJSON()示例

以上**的简写。ajax()** 方法，两者都在做同样的任务。

```java
 $.getJSON("
   http://feeds.delicious.com/v2/json/urlinfo/data?url="+url+"&callback=?",

   function(data) {

   var count = 0;
   if (data.length > 0) {
	count = data[0].total_posts;
   }
   $("#delicious_result").text(count + ' Saved');

}); 
```

## 你自己试试

在本例中，在文本框中输入 URL，然后点击按钮以获得 Delicious 中书签的总数。

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.4.2.min.js"></script>
</head>

<body>
<h1>Get Delicious bookmark count with jQuery</h1>

URL : <input type='text' id='url' size='50' value='http://www.google.com' />
<br/><br/>
<h2>Delicious count : <span id="delicious_result"></span></h2>

<button id="delicious">Get Delicious Count (.Ajax)</button>
<button id="delicious2">Get Delicious Count (.getJSON)</button>
<script type="text/javascript">

$('#delicious').click(function(){

 $("#delicious_result").text("Loading......");

 var url = $('#url').val();

 $.ajax({ 
 type: "GET",
 dataType: "json",
 url: "http://feeds.delicious.com/v2/json/urlinfo/data?url="+url+"&amp;callback=?",
 success: function(data){

	var count = 0;
	if (data.length > 0) {
		count = data[0].total_posts;
	}
	$("#delicious_result").text(count + ' Saved');

   }
  });
});

$('#delicious2').click(function(){

 $("#delicious_result").text("Loading......");

 var url = $('#url').val();

 $.getJSON("
    http://feeds.delicious.com/v2/json/urlinfo/data?url="+url+"&callback=?",

 function(data) {

	var count = 0;
	if (data.length > 0) {
		count = data[0].total_posts;
	}
	$("#delicious_result").text(count + ' Saved');

  });	
});
</script>

</body>
</html> 
```

[http://web.archive.org/web/20220204001715if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-get-delicious-bookmark-count-example.html](http://web.archive.org/web/20220204001715if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-get-delicious-bookmark-count-example.html)

[Try Demo](http://web.archive.org/web/20220204001715/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-get-delicious-bookmark-count-example.html)

## 参考

1.  [http://delicious.com/help/feeds](http://web.archive.org/web/20220204001715/http://delicious.com/help/feeds)
2.  [http://API . jquery . com/jquery . getjson/](http://web.archive.org/web/20220204001715/https://api.jquery.com/jQuery.getJSON/)

<input type="hidden" id="mkyong-current-postId" value="5303">