# 警告:JSF1063:警告！将不可序列化的属性值设置到 HttpSession 中

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/warning-jsf1063-warning-setting-non-serializable-attribute-value-into-httpsession/>

## 问题

在 JSF 2.0 web 应用程序中，在服务器初始化期间，它会显示以下警告消息

> 警告:JSF1063:警告！将不可序列化的属性值设置到 HttpSession
> (key: user，value class: com.mkyong.UserBean)。

**UserBean.java**

```java
 package com.mkyong;

@ManagedBean(name="user")
@SessionScoped
public class UserBean{

	//...
} 
```

 ## 解决办法

“UserBean”不可序列化。要消除这个警告消息，只需让这个 bean 实现 **java.io.Serializable** 接口。

**UserBean.java**

```java
 package com.mkyong;

import java.io.Serializable;

@ManagedBean(name="user")
@SessionScoped
public class UserBean implements Serializable{

	//...
} 
```

[jsf2](http://web.archive.org/web/20190116170620/http://www.mkyong.com/tag/jsf2/)![](img/6a55ff97b6ecc1d6bfc25f29a165d377.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190116170620/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="7274">







