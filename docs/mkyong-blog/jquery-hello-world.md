# jQuery Hello World

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-hello-world/>

> jQuery 是一个快速而简洁的 JavaScript 库，它简化了 HTML 文档遍历、事件处理、动画和 Ajax 交互，有助于快速的 web 开发。jQuery 旨在改变您编写 JavaScript 的方式。

jQuery 是新技术？肯定不是，我认为 jQuery 是一种简化客户端 web 开发的新方法，就像 HTML 遍历 DOM 和 Ajax 操作一样。iQuery 是由一个年轻的孩子 John Resig 创建的，请访问他的网站，看看这个 jQuery 家伙长什么样~

让我们通过一个 jQuery Hello World 示例来理解它是如何工作的。

## 1.下载 jQuery 库

jQuery 只是一个 20+kb 的小 JavaScript 文件(如 jquery-1.2.6.min.js)，你可以从 [jQuery 官网](http://web.archive.org/web/20220618072947/https://jquery.com/)下载。。

## 2.HTML + jQuery

创建一个简单的 HTML 文件(test.html)，并将下载的 jQuery 库作为普通的 JavaScript 文件包含在内。js)。

```java
 <html>
<head>
<title>jQuery Hello World</title>

<script type="text/javascript" src="jquery-1.2.6.min.js"></script>

</head>

<body>

<script type="text/javascript">

$(document).ready(function(){
 $("#msgid").html("This is Hello World by JQuery");
});

</script>

This is Hello World by HTML

<div id="msgid">
</div>

</body>
</html> 
```

## 3.演示

![](img/96a52944cd5a2d12996c30f055b117ae.png "jQuery-Hello-World")

＄()表示一个 jQuery 语法，下面的脚本表示当 DOM 元素准备好或完全加载时，执行 jQuery 脚本来动态创建一个消息，并将其附加到 html 标记 id“msgid”。

```java
 $(document).ready(function(){
 $("#msgid").html("This is Hello World by JQuery");
}); 
```

<input type="hidden" id="mkyong-current-postId" value="324">