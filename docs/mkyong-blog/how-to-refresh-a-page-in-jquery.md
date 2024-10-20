# 如何在 jQuery 中刷新页面？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-refresh-a-page-in-jquery/>

要用 jQuery 刷新或重新加载页面，可以使用"**location . reload()；**”命令。

```java
 <html>
<head>
<title>Refresh a page in jQuery</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

</head>

<body>

<button id="PageRefresh">Refresh a Page in jQuery</button>

<script type="text/javascript">

	$('#PageRefresh').click(function() {

    	      location.reload();

	});

</script>

</body>
</html> 
```

Tags : [jquery](http://web.archive.org/web/20210411191225/https://mkyong.com/tag/jquery/) [refresh page](http://web.archive.org/web/20210411191225/https://mkyong.com/tag/refresh-page/)<input type="hidden" id="mkyong-current-postId" value="5011">