# 如何在 Java web 服务客户机中绕过证书检查

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/webservices/jax-ws/how-to-bypass-certificate-checking-in-a-java-web-service-client/>

在 Java web 服务开发环境中，开发人员总是使用`keytool`来生成测试证书。在进行客户端测试时，web 服务测试客户端经常会遇到以下错误消息:

1.  [Java . security . cert . certificate 异常:找不到与本地主机匹配的名称](http://web.archive.org/web/20190303105144/http://www.mkyong.com/webservices/jax-ws/java-security-cert-certificateexception-no-name-matching-localhost-found/)
2.  [SunCertPathBuilderException:无法找到请求目标的有效认证路径](http://web.archive.org/web/20190303105144/http://www.mkyong.com/webservices/jax-ws/suncertpathbuilderexception-unable-to-find-valid-certification-path-to-requested-target/)

这里有一个源代码，是我从马丁·卡琳的书《Java Web Services:启动并运行，第一版》中复制过来的。一个在测试环境中非常**有用的代码唯一**，推荐学习并收藏备查:)

**Warning**
Don’t try it at production environment, unless you have a very solid reason to by pass all the certificate checking. Ans if yes, why are you still using SSL connection? :)

```java
 package com.mkyong.client;

import java.net.MalformedURLException;
import java.net.URL;
import java.security.KeyManagementException;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;
import java.security.cert.Certificate;
import java.security.cert.X509Certificate;
import java.io.*;

import javax.net.ssl.HostnameVerifier;
import javax.net.ssl.HttpsURLConnection;
import javax.net.ssl.SSLContext;
import javax.net.ssl.SSLPeerUnverifiedException;
import javax.net.ssl.SSLSession;
import javax.net.ssl.TrustManager;
import javax.net.ssl.X509TrustManager;

public class HttpsClient{

  public static void main(String[] args)
  {
     new HttpsClient().testIt();
  }

  private TrustManager[ ] get_trust_mgr() {
     TrustManager[ ] certs = new TrustManager[ ] {
        new X509TrustManager() {
           public X509Certificate[ ] getAcceptedIssuers() { return null; }
           public void checkClientTrusted(X509Certificate[ ] certs, String t) { }
           public void checkServerTrusted(X509Certificate[ ] certs, String t) { }
         }
      };
      return certs;
  }

  private void testIt(){
     String https_url = "https://localhost:8443/HelloWorld/hello?wsdl";
     URL url;
     try {

	    // Create a context that doesn't check certificates.
            SSLContext ssl_ctx = SSLContext.getInstance("TLS");
            TrustManager[ ] trust_mgr = get_trust_mgr();
            ssl_ctx.init(null,                // key manager
                         trust_mgr,           // trust manager
                         new SecureRandom()); // random number generator
            HttpsURLConnection.setDefaultSSLSocketFactory(ssl_ctx.getSocketFactory());

	    url = new URL(https_url);
	    HttpsURLConnection con = (HttpsURLConnection)url.openConnection();

	    // Guard against "bad hostname" errors during handshake.
            con.setHostnameVerifier(new HostnameVerifier() {
                public boolean verify(String host, SSLSession sess) {
                    if (host.equals("localhost")) return true;
                    else return false;
                }
            });

	    //dumpl all cert info
	    print_https_cert(con);

	    //dump all the content
	    print_content(con);

	 } catch (MalformedURLException e) {
		e.printStackTrace();
	 } catch (IOException e) {
		e.printStackTrace();
	 }catch (NoSuchAlgorithmException e) {
		e.printStackTrace();
	 }catch (KeyManagementException e) {
		e.printStackTrace();
      }	
   }

  private void print_https_cert(HttpsURLConnection con){
     if(con!=null){

     try {

	System.out.println("Response Code : " + con.getResponseCode());
	System.out.println("Cipher Suite : " + con.getCipherSuite());
	System.out.println("\n");

	Certificate[] certs = con.getServerCertificates();
	for(Certificate cert : certs){
	  System.out.println("Cert Type : " + cert.getType());
	  System.out.println("Cert Hash Code : " + cert.hashCode());
	  System.out.println("Cert Public Key Algorithm : " + cert.getPublicKey().getAlgorithm());
	  System.out.println("Cert Public Key Format : " + cert.getPublicKey().getFormat());
	  System.out.println("\n");
	}

     } catch (SSLPeerUnverifiedException e) {
	  e.printStackTrace();
     } catch (IOException e){
	  e.printStackTrace();
     }	   
   }		
  }

  private void print_content(HttpsURLConnection con){
    if(con!=null){

    try {

	System.out.println("****** Content of the URL ********");

	BufferedReader br = 
		new BufferedReader(
			new InputStreamReader(con.getInputStream()));

	String input;

	while ((input = br.readLine()) != null){
	   System.out.println(input);
	}
	br.close();

     } catch (IOException e) {
	e.printStackTrace();
     }		
   }
  }
} 
```

[jax-ws](http://web.archive.org/web/20190303105144/http://www.mkyong.com/tag/jax-ws/) [web services](http://web.archive.org/web/20190303105144/http://www.mkyong.com/tag/web-services/)![](img/b9101bf7a141404684e92c4443ba135f.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190303105144/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="7918">







