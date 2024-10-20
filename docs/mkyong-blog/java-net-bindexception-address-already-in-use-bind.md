# JAX-WS–Java . net . bind 异常:地址已在使用中:bind

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/webservices/jax-ws/java-net-bindexception-address-already-in-use-bind/>

用 JAX-WS 开发一个 Java web 服务开发，并发布一个端点…

```java
 public static void main(String[] args) {
   Endpoint.publish("http://localhost:8080/ws/hello", new WallStreetImpl());
} 
```

## 1.问题

它会显示以下错误消息。

```java
 Exception in thread "main" com.sun.xml.internal.ws.server.ServerRtException: 
	Server Runtime Error: java.net.BindException: Address already in use: bind
	...
Caused by: java.net.BindException: Address already in use: bind
	at sun.nio.ch.Net.bind(Native Method)
	... 
```

 ## 2.解决办法

一个非常常见的错误消息，它意味着地址(通常是端口号)已经被另一个应用程序使用。

**要修复它**，请更改端点端口号:

```java
 public static void main(String[] args) {
   Endpoint.publish("http://localhost:1234/ws/hello", new WallStreetImpl());
} 
```

 ## 参考

1.  [Linux–哪个应用程序正在使用端口 8080](http://web.archive.org/web/20190201025100/http://www.mkyong.com/linux/linux-which-application-is-using-port-8080/)

[jax-ws](http://web.archive.org/web/20190201025100/http://www.mkyong.com/tag/jax-ws/) [web services](http://web.archive.org/web/20190201025100/http://www.mkyong.com/tag/web-services/)







