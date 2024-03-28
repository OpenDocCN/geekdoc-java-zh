# 结果计数

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/countlimit.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/countlimit.html)

有时，查询可能产生太多答案，你希望限制返回的答案数量。你可以通过使用计数限制搜索控件来实现这一点。默认情况下，搜索没有计数限制 - 它将返回它找到的所有答案。要设置搜索的计数限制，请将数字传递给[`SearchControls.setCountLimit()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setCountLimit-long-)。

`以下示例`将计数限制设置为 1。

```java
// Set the search controls to limit the count to 1
SearchControls ctls = new SearchControls();
ctls.setCountLimit(1);

```

如果程序尝试获取超过计数限制数量的结果，那么将抛出[`SizeLimitExceededException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/SizeLimitExceededException.html)。因此，如果程序设置了计数限制，则应该区分此异常和其他[`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html)，或者跟踪计数限制并不请求超过该数量的结果。

指定搜索的计数限制是控制应用程序消耗资源（如内存和网络带宽）的一种方式。控制消耗资源的其他方法包括缩小你的搜索过滤器（更具体地说明你要寻找什么）、在适当的上下文中开始搜索，并使用适当的范围。
