# 找不到基本名称 xxx，区域设置 en_US 的包

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/cant-find-bundle-for-base-name-xxx-locale-en_us/>

## 问题

在 JSF web 应用中，在应用层加载一个消息包，如下:
**faces-config.xml**

```java
 <?xml version="1.0" encoding="UTF-8"?>
<faces-config

    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
    http://java.sun.com/xml/ns/javaee/web-facesconfig_2_0.xsd"
    version="2.0">
     <application>
	<message-bundle>
		com.mkyong.payment_error
	</message-bundle>
     </application>
</faces-config> 
```

页面渲染时，遇到“**找不到 base name com . mkyong . payment _ error，locale en_US** 的 bundle”？

 ## 解决办法

显然，包或属性文件(**com . mkyong . payment _ error . properties**)丢失了，请确保名称匹配并正确放置在资源文件夹中。

**For Eclipse User**
This problem is usually happened in the Eclipse IDE environment, where it doesn’t copy the “**.properties**” file extension by default. So, just make sure the properties file is existed in the run time “classes” folder and can be located by your web application.[jsf2](http://web.archive.org/web/20190224212715/http://www.mkyong.com/tag/jsf2/)![](img/3e072f25f11cc9085d56f53f648f0401.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224212715/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="7481">







