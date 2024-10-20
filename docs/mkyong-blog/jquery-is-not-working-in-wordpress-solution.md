# JQuery 在 WordPress-Solution 中不工作

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-is-not-working-in-wordpress-solution/>

> 由于 WordPress 2 . x 版，jQuery 是一个内置的 Javascript 库，没有必要明确地将 jQuery 库包含到 WordPress 中。

## 问题

jQuery 在 WordPress 插件编写中不工作？当您尝试测试一个简单的 jQuery 效果时，如下所示

```java
 $(document).ready(function(){
  alert('test');
}); 
```

它只是不工作，没有警告消息框弹出。相同的脚本在单个 HTML 页面中正常运行。这是怎么回事？jQuery 和 WordPress 之间有互操作性问题吗？

## 解决办法

在 WordPress 中， **$()** 语法总是被其他脚本库使用，并导致冲突问题和无法调用 jQuery 函数。你应该使用 **jQuery()** 来代替…

```java
 jQuery(document).ready(function(){
  alert('test');
}); 
```

或者，您可以使用 **noConflict()** …

```java
 $j=jQuery.noConflict();

// Use jQuery via $j(...)
$j(document).ready(function(){
  alert('test');
}); 
```

*P . S**jquery . no conflict()；【http://wordpress.org/support/topic/141394】—***

永远不要在 WordPress 插件中使用 jQuery 便捷函数 **$()** 。你必须使用 **jQuery()** 或 **jQuery.noConflict()** 在 jQuery 和 WordPress 之间工作。

<input type="hidden" id="mkyong-current-postId" value="393">