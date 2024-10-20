# 如何在 Maven 中启用代理设置

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/maven/how-to-enable-proxy-setting-in-maven/>

要在 Maven 中启用代理访问，请在`{MAVEN_HOME}/conf/settings.xml`中定义代理服务器细节

**Note**
There is a high chance your company is set up an HTTP proxy server to stop user connecting to the Internet directly. If you are behind a proxy, Maven will fail to download the project dependencies.

*PS 用 Maven 3.6 测试过*

## 1.代理访问

1.打开 Maven `settings.xml`，找到`proxies`标签:

{MAVEN_HOME}/conf/settings.xml

```java
 <!-- proxies
   | This is a list of proxies which can be used on this machine to connect to the network.
   | Unless otherwise specified (by system property or command-line switch), the first proxy
   | specification in this list marked as active will be used.
   |-->
  <proxies>
    <!-- proxy
     | Specification for one proxy, to be used in connecting to the network.
     |
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>proxyuser</username>
      <password>proxypass</password>
      <host>proxy.host.net</host>
      <port>80</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>
    -->
  </proxies> 
```

1.2 定义了如下的代理服务器设置:

```java
 <proxies>
    <!-- proxy
     | Specification for one proxy, to be used in connecting to the network.
     |
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>proxyuser</username>
      <password>proxypass</password>
      <host>proxy.host.net</host>
      <port>80</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>
    -->

	<proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>mkyong</username>
      <password>password</password>
      <host>proxy.mkyong.com</host>
      <port>8888</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>

  </proxies> 
```

1.3 完成后，Apache Maven 应该能够通过代理服务器连接到互联网。

## 参考

1.  [配置代理](http://web.archive.org/web/20221023054402/https://maven.apache.org/guides/mini/guide-proxies.html)

<input type="hidden" id="mkyong-current-postId" value="2114">