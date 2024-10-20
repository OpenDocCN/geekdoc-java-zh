# Java . security . cert . certificate 异常:找不到与本地主机匹配的名称

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/webservices/jax-ws/java-security-cert-certificateexception-no-name-matching-localhost-found/>

## 问题

配置了 [Tomcat 来支持 SSL](http://web.archive.org/web/20190223080735/http://www.mkyong.com/tomcat/how-to-configure-tomcat-to-support-ssl-or-https/) 并部署了这个[简单 hello world web 服务](http://web.archive.org/web/20190223080735/http://www.mkyong.com/webservices/jax-ws/jax-ws-hello-world-example/)。并通过 SSL 连接使用以下客户端连接到已部署的 web 服务:

```java
 package com.mkyong.client;

import java.net.URL;
import javax.xml.namespace.QName;
import javax.xml.ws.Service;

import com.mkyong.ws.HelloWorld;

public class HelloWorldClient{

	public static void main(String[] args) throws Exception {

	URL url = new URL("https://localhost:8443/HelloWorld/hello?wsdl");
        QName qname = new QName("http://ws.mkyong.com/", "HelloWorldImplService");

        Service service = Service.create(url, qname);
        HelloWorld hello = service.getPort(HelloWorld.class);
        System.out.println(hello.getHelloWorldAsString());

    }
} 
```

命中“**未找到与本地主机匹配的名称**”异常:

```java
 Caused by: javax.net.ssl.SSLHandshakeException: 
    java.security.cert.CertificateException: No name matching localhost found
	at com.sun.net.ssl.internal.ssl.Alerts.getSSLException(Alerts.java:174)
	at com.sun.net.ssl.internal.ssl.SSLSocketImpl.fatal(SSLSocketImpl.java:1611)
	at com.sun.net.ssl.internal.ssl.Handshaker.fatalSE(Handshaker.java:187)
	at com.sun.net.ssl.internal.ssl.Handshaker.fatalSE(Handshaker.java:181)
	......
Caused by: java.security.cert.CertificateException: No name matching localhost found
	at sun.security.util.HostnameChecker.matchDNS(HostnameChecker.java:210)
	at sun.security.util.HostnameChecker.match(HostnameChecker.java:77)
	...... 
```

 ## 解决办法

这个问题和解决方案在这篇[文章](http://web.archive.org/web/20190223080735/http://docs.sun.com/app/docs/doc/820-1072/6ncp48v40?a=view#ahicy)中有很好的解释，您可以为您的“ *localhost* 开发环境使用一个传输安全(SSL)解决方案。

要修复它，添加一个`javax.net.ssl.HostnameVerifier()`方法来覆盖现有的主机名验证器，如下所示:

```java
 package com.mkyong.client;

import java.net.URL;
import javax.xml.namespace.QName;
import javax.xml.ws.Service;

import com.mkyong.ws.HelloWorld;

public class HelloWorldClient{

	static {
	    //for localhost testing only
	    javax.net.ssl.HttpsURLConnection.setDefaultHostnameVerifier(
	    new javax.net.ssl.HostnameVerifier(){

	        public boolean verify(String hostname,
	                javax.net.ssl.SSLSession sslSession) {
	            if (hostname.equals("localhost")) {
	                return true;
	            }
	            return false;
	        }
	    });
	}

	public static void main(String[] args) throws Exception {

	URL url = new URL("https://localhost:8443/HelloWorld/hello?wsdl");
        QName qname = new QName("http://ws.mkyong.com/", "HelloWorldImplService");

        Service service = Service.create(url, qname);
        HelloWorld hello = service.getPort(HelloWorld.class);
        System.out.println(hello.getHelloWorldAsString());

    }
} 
```

*输出*

```java
 Hello World JAX-WS 
```

它现在工作正常。

[jax-ws](http://web.archive.org/web/20190223080735/http://www.mkyong.com/tag/jax-ws/) [web services](http://web.archive.org/web/20190223080735/http://www.mkyong.com/tag/web-services/)![](img/fc7d2588893b5063bdd27c47442feb3b.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190223080735/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="7908">







