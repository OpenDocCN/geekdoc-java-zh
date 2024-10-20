# Struts 2 FilterDispatcher 和 StrutsPrepareAndExecuteFilter 的区别？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/difference-between-struts-2-filterdispatcher-and-strutsprepareandexecutefilter/>

## 问题

在 Struts2 开发中，很多人问为什么有些将 filter 类声明为“**filter dispatcher**”；还有人声明“**strutsprepareendexecutefilter**”？但是两者都工作得很好，有什么不同呢？

*1。FilterDispatcher 示例*

```java
 ...
<filter>
   <filter-name>struts2</filter-name>
   <filter-class>
     org.apache.struts2.dispatcher.FilterDispatcher
   </filter-class>
</filter>

<filter-mapping>
   <filter-name>struts2</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
... 
```

*2。StrutsPrepareAndExecuteFilter 示例*

```java
 ...
<filter>
  <filter-name>struts2</filter-name>
  <filter-class>
        org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
  </filter-class>
</filter>

<filter-mapping>
   <filter-name>struts2</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
... 
```

 ## 回答

**FilterDispatcher** ( `org.apache.struts2.dispatcher.FilterDispatcher`)用于早期的 Struts2 开发中，**从 Struts 2.1.3** 开始就不再使用了。

如果您使用的是 Struts 版本> = 2.1.3，那么总是建议升级新的过滤器类—**strutsprepareendexecutefilter**(`org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter`)。

 ## 参考

1.  [FilterDispatcher 文档](http://web.archive.org/web/20190225101604/http://struts.apache.org/2.1.8/struts2-core/apidocs/org/apache/struts2/dispatcher/FilterDispatcher.html)
2.  [strutsprepareendexecutefilter 文档](http://web.archive.org/web/20190225101604/http://struts.apache.org/2.1.8/struts2-core/apidocs/org/apache/struts2/dispatcher/ng/filter/StrutsPrepareAndExecuteFilter.html)

[struts2](http://web.archive.org/web/20190225101604/http://www.mkyong.com/tag/struts2/)







