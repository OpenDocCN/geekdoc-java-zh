# 将 Spring XML 文件导入@Configuration

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring3/import-spring-xml-files-into-configuration/>

将 XML 配置混合到 Spring `@Configuration`中是很常见的，因为开发人员已经习惯了 XML 名称空间。在 Spring 中，您可以使用`@ImportResource`将 Spring XML 配置文件导入到`@Configuration`中:

AppConfig.java

```java
 import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.ImportResource;

@Configuration
@ImportResource("classpath:/config/spring.xml")
public class AppConfig {

} 
```

另一个例子

AppConfig.java

```java
 import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.ImportResource;
import org.springframework.context.annotation.Import;

@Configuration
@Import({ AppConfigWeb.class })
@ImportResource("classpath:/config/spring.xml")
public class AppConfig {

} 
```

*P.S `@ImportResource`自春季 3.0* 开始发售

[spring](http://web.archive.org/web/20190309055132/http://www.mkyong.com/tag/spring/) [spring config](http://web.archive.org/web/20190309055132/http://www.mkyong.com/tag/spring-config/) [spring3](http://web.archive.org/web/20190309055132/http://www.mkyong.com/tag/spring3/)![](img/97c4bd8bfa5c0ef48be5248bdcbe10c8.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190309055132/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="13614">







