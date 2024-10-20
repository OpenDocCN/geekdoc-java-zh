# 如何检查 jQuery 库是否加载？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-check-if-jquery-library-is-loaded/>

要检查 jQuery 库是否已加载，请使用以下 JavaScript 代码:

```java
 if (typeof jQuery != 'undefined') {

    alert("jQuery library is loaded!");

}else{

    alert("jQuery library is not found!");

} 
```

## 另类？

在一些博客和论坛中提到了以下替代代码:

```java
 if (jQuery) {  

   alert("jQuery library is loaded!");

} else {

   alert("jQuery library is not found!");

} 
```

然而，这是行不通的，当没有加载 jQuery 库时，这段代码将失败( **jQuery 没有定义**)，并且警告消息将永远不会执行。

最后，总是推荐第一种 jQuery 检查方法。

<input type="hidden" id="mkyong-current-postId" value="5183">