# 如何在 Wicket 中使用 AjaxLazyLoadPanel

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/how-do-use-ajaxlazyloadpanel-in-wicket/>

*Ajax azionloadpanel 定义:*

> 一个面板，你可以懒加载另一个面板。如果您有一个创建起来相当繁重的面板/组件，并且您首先想要向用户显示页面并在面板准备好时替换它，那么可以使用这种方法。

这个功能真的让人印象深刻。这里我们向你展示如何将一个普通的面板转换成这个强大的`AjaxLazyLoadPanel`。

## 原始面板

普通边门面板。

```java
 add(new PricePanel("price")); 
```

 ## 惰性负载面板

转换到 Wicket `AjaxLazyLoadPanel`。

```java
 add(new AjaxLazyLoadPanel("price")
{
  @Override
  public Component getLazyLoadComponent(String id)
  {
       return PricePanel(id);
  }
}); 
```

搞定了，现在 PricePanel 有懒加载效果了。尼斯（法国城市名）

 ## 当心！

这个 **AjaxLazyLoadPanel** 的一个缺点是它不包含后备版本。如果浏览器的 JavaScript 被禁用，懒惰图像将永远保持加载。

这里有一个恶作剧。

1.将以下代码放入 Wicket 的应用程序类中

```java
 protected void init() {		
		getRequestCycleSettings().setGatherExtendedBrowserInfo(true);
} 
```

2.检查一下

```java
 WebClientInfo clientInfo = (WebClientInfo)WebRequestCycle.get().getClientInfo();
if(clientInfo.getProperties().isJavaEnabled()){
add(new AjaxLazyLoadPanel("price")
{
  @Override
  public Component getLazyLoadComponent(String id)
  {
       return PricePanel("price");
  }
});
}else{
  add(new PricePanel("price"));
} 
```

如果浏览器支持 JavaScript，上述函数将运行 AjaxLazyLoadPanel 函数，否则委托给正常请求。

## 参考

1.  [如何在 Wicket 中检测浏览器是否支持 JavaScript](http://web.archive.org/web/20190304031454/http://www.mkyong.com/wicket/how-do-detect-browser-javascript-or-ajax-disabled-in-wicket/)

[lazy load](http://web.archive.org/web/20190304031454/http://www.mkyong.com/tag/lazy-load/) [wicket](http://web.archive.org/web/20190304031454/http://www.mkyong.com/tag/wicket/)







