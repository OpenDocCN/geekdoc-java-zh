# 在 Wicket 中检测浏览器是否支持 javascript 或 ajax？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/how-do-detect-browser-javascript-or-ajax-disabled-in-wicket/>

Wicket 内置了许多 ajax 后备组件，如果 Ajax 不可用或 Javascript 被禁用，这些组件就会降级为普通请求。一些很好的例子是 AjaxFallbackButton，AjaxFallbackLink 等。

**Note**
However there are some components that didn’t implement the fallback behavior, for instance AjaxLazyLoadPanel, it will keep display the Wicket’s default image forever if JavaScript is disabled.

## 解决办法

实际上，Wicket 有一个内置机制来检测浏览器是否支持 JavaScript。但是，Wicket 默认关闭了这个功能是有原因的。

 ## 1.收集您的浏览器信息

在 Wicket 应用程序中，用**setgathereextendedbrowserinfo(true)**覆盖`init()`，它告诉 Wicket 从浏览器收集额外信息。

```java
 protected void init() {		
		getRequestCycleSettings().setGatherExtendedBrowserInfo(true);
	} 
```

 ## 2.检测 JavaScript

现在，您可以像这样检测浏览器是否支持 JavaScript:

```java
 WebClientInfo clientInfo = (WebClientInfo)WebRequestCycle.get().getClientInfo();
if(clientInfo.getProperties().isJavaEnabled()){
  //Javascript is enable
}else{
 //Javascript is enable
} 
```

## 说明

当用户请求一个页面时，Wicket 将在客户端浏览器中执行一个简单的 JavaScript 重定向测试，以测试浏览器的 JavaScript 是否受支持，并将其保存到用户的会话中。执行重定向测试后，Wicket 将重定向到最初请求的页面。整个过程非常快，但是你会看到一闪而过的空白页。这就是 Wicket 默认关闭它的原因。黑色页面的闪烁真的很讨厌，当你看到这种闪烁页面时，你可能会认为这个网站是一个钓鱼网站:)，你的客户也是如此。

Wicket 重定向测试页面是通用的，适用于所有情况，但是空白页面的闪现确实是不可接受的，至少对我来说是这样。

[ajax](http://web.archive.org/web/20190112014413/http://www.mkyong.com/tag/ajax/) [browser](http://web.archive.org/web/20190112014413/http://www.mkyong.com/tag/browser/) [javascript](http://web.archive.org/web/20190112014413/http://www.mkyong.com/tag/javascript/) [wicket](http://web.archive.org/web/20190112014413/http://www.mkyong.com/tag/wicket/)







