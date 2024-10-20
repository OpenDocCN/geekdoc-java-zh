# 如何改变 Wicket URL bookmarkablePage 结构？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/how-do-change-wicket-url-bookmarkablepage-structure-url-mounting/>

## 问题

默认情况下，Wicket 生成的 URL 又长又丑，包含页面的全限定类名。它看起来像这样:

```java
 http://localhost:8080/mkyong?wicket:bookmarkablePage=:com.mkyong.page.ResultPage&url=google.com 
```

**Note**
What the heck is “**wicket:bookmarkablePage**” in URL, and why Wicket generated such an ugly URL structure? After deployed Wicket application to client site, many clients’ emails sending in and complaint about the ugly bookmarkablePage URL structure. It’s just sound wired and doesn’t make sense at all, what is the advantages of this? Are you going to ask my visitor to bookmark this ugly URL address?. ## 解决办法

幸运的是，Wicket 提供了" **URL mounting** "特性，将难看的 URL 可加书签页面隐藏到我们应用程序中的特定路径。

要解决这个问题，请更改 Wicket 应用程序类中默认的丑陋 URL 结构，" **init()** "方法，如下所示

```java
 import org.apache.wicket.protocol.http.WebApplication;
import org.apache.wicket.request.target.coding.QueryStringUrlCodingStrategy;
import com.mkyong.user.SuccessPage;

public class WicketApplication extends WebApplication {

	@Override
	protected void init() {
		super.init();
		mount(new QueryStringUrlCodingStrategy("result",ResultPage.class));
	}
} 
```

使用**QueryStringUrlCodingStrategy()**将“ **ResultPage.class** ”页面挂载到一个整洁友好的 URL 结构“**结果**，见输出:

```java
 http://localhost:8080/mkyong/result?url=google.com 
```

[url structure](http://web.archive.org/web/20190114230057/http://www.mkyong.com/tag/url-structure/) [wicket](http://web.archive.org/web/20190114230057/http://www.mkyong.com/tag/wicket/)![](img/f5b833fe89f5fe145fb1281ff2c2a7f6.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190114230057/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="1181">







