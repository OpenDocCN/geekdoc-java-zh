# Spring Boot–在嵌入式 Tomcat 中配置 maxSwallowSize

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-configure-maxswallowsize-in-embedded-tomcat/>

在 Spring Boot，你不能通过[通用应用属性](http://web.archive.org/web/20190309055300/http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#common-application-properties)配置嵌入式 Tomcat `maxSwallowSize`，没有像`server.tomcat.*.maxSwallowSize`这样的选项

## 解决办法

要修复它，您需要声明一个`TomcatEmbeddedServletContainerFactory` bean，并像这样配置`maxSwallowSize`:

```java
 //...
import org.apache.coyote.http11.AbstractHttp11Protocol;
import org.springframework.boot.context.embedded.tomcat.TomcatConnectorCustomizer;
import org.springframework.boot.context.embedded.tomcat.TomcatEmbeddedServletContainerFactory;

    private int maxUploadSizeInMb = 10 * 1024 * 1024; // 10 MB

    @Bean
    public TomcatEmbeddedServletContainerFactory tomcatEmbedded() {

        TomcatEmbeddedServletContainerFactory tomcat = new TomcatEmbeddedServletContainerFactory();

        tomcat.addConnectorCustomizers((TomcatConnectorCustomizer) connector -> {

            // connector other settings...

            // configure maxSwallowSize
            if ((connector.getProtocolHandler() instanceof AbstractHttp11Protocol<?>)) {
                // -1 means unlimited, accept bytes
                ((AbstractHttp11Protocol<?>) connector.getProtocolHandler()).setMaxSwallowSize(-1);
            }

        });

        return tomcat;

    } 
```

 ## 参考

1.  [Spring 文件上传和连接重置问题](http://web.archive.org/web/20190309055300/http://www.mkyong.com/spring/spring-file-upload-and-connection-reset-issue/)
2.  [常用应用属性](http://web.archive.org/web/20190309055300/http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#common-application-properties)
3.  [Spring Boot 文件上传错误处理(Japanase)](http://web.archive.org/web/20190309055300/https://www.agilegroup.co.jp/technote/springboot-fileupload-error-handling.html)

[connection reset](http://web.archive.org/web/20190309055300/http://www.mkyong.com/tag/connection-reset/) [file upload](http://web.archive.org/web/20190309055300/http://www.mkyong.com/tag/file-upload/) [spring boot](http://web.archive.org/web/20190309055300/http://www.mkyong.com/tag/spring-boot/) [tomcat](http://web.archive.org/web/20190309055300/http://www.mkyong.com/tag/tomcat/)![](img/0efdae9172393ecbeacc09e2ce45232b.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190309055300/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="14377">







