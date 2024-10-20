# 在 Wicket 中向 HTML 标签动态添加属性

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/how-to-dynamic-add-attribute-to-a-html-tag-in-wicket/>

在 Wicket 中，您可以轻松地访问或操作 HTML 标记。比方说，您有一个 HTML 文本框组件，由一个 div 标记包装，如果文本框验证失败，div 标记应该以错误颜色突出显示。

在上面的例子中，你可以实现" **AbstractBehavior** "类，来动态地给 HTML 标签添加属性。请参见以下示例:

*原始 HTML*

```java

    Hello ~ Wicket leaning curve is high, do you?

```

*用 Wicket AbstractBehavior 修改*

```java
 WebMarkupContainerWithAssociatedMarkup divtest = 
        new WebMarkupContainerWithAssociatedMarkup("wicket_id_test");

    //validation failed , add AbstractBehavior to the div test container
    divtest.add(new AbstractBehavior() {

	    public void onComponentTag(Component component, ComponentTag tag) {
			tag.put("style", "background-color:red");
	    }
	}); 
```

*结果如下:*

```java

    Hello ~ this is testing for adding attribute into above tag in Wicket ~

```

[wicket](http://web.archive.org/web/20190304031820/http://www.mkyong.com/tag/wicket/)![](img/869fc80361724545cb4733eaa5f4d45b.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304031820/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="1749">







