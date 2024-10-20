# 带有 Jersey 客户端的 RESTful Java 客户端

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/webservices/jax-rs/restful-java-client-with-jersey-client/>

本教程向您展示了如何使用 **Jersey 客户端 API**来创建一个 RESTful Java 客户端，以执行在这个“ [Jersey + Json](http://web.archive.org/web/20190309050309/http://www.mkyong.com/webservices/jax-rs/json-example-with-jersey-jackson/) ”示例中创建的“ **GET** ”和“**POST**”REST 服务请求。

## 1.Jersey 客户端依赖性

为了使用 Jersey 客户端 API，在您的`pom.xml`文件中声明“ **jersey-client.jar** ”。

*文件:pom.xml*

```java
 <dependency>
		<groupId>com.sun.jersey</groupId>
		<artifactId>jersey-client</artifactId>
		<version>1.8</version>
	</dependency> 
```

 ## 2.获取请求

回顾上次休息服务。

```java
 @Path("/json/metallica")
public class JSONService {

	@GET
	@Path("/get")
	@Produces(MediaType.APPLICATION_JSON)
	public Track getTrackInJSON() {

		Track track = new Track();
		track.setTitle("Enter Sandman");
		track.setSinger("Metallica");

		return track;

	}
	//... 
```

Jersey 客户端发送一个“GET”请求并打印出返回的 json 数据。

```java
 package com.mkyong.client;

import com.sun.jersey.api.client.Client;
import com.sun.jersey.api.client.ClientResponse;
import com.sun.jersey.api.client.WebResource;

public class JerseyClientGet {

  public static void main(String[] args) {
	try {

		Client client = Client.create();

		WebResource webResource = client
		   .resource("http://localhost:8080/RESTfulExample/rest/json/metallica/get");

		ClientResponse response = webResource.accept("application/json")
                   .get(ClientResponse.class);

		if (response.getStatus() != 200) {
		   throw new RuntimeException("Failed : HTTP error code : "
			+ response.getStatus());
		}

		String output = response.getEntity(String.class);

		System.out.println("Output from Server .... \n");
		System.out.println(output);

	  } catch (Exception e) {

		e.printStackTrace();

	  }

	}
} 
```

输出…

```java
 Output from Server .... 

{"singer":"Metallica","title":"Enter Sandman"} 
```

 ## 3.发布请求

回顾上次休息服务。

```java
 @Path("/json/metallica")
public class JSONService {

	@POST
	@Path("/post")
	@Consumes(MediaType.APPLICATION_JSON)
	public Response createTrackInJSON(Track track) {

		String result = "Track saved : " + track;
		return Response.status(201).entity(result).build();

	}
	//... 
```

Jersey 客户端发送一个带有 json 数据的“POST”请求，并打印出返回的输出。

```java
 package com.mkyong.client;

import com.sun.jersey.api.client.Client;
import com.sun.jersey.api.client.ClientResponse;
import com.sun.jersey.api.client.WebResource;

public class JerseyClientPost {

  public static void main(String[] args) {

	try {

		Client client = Client.create();

		WebResource webResource = client
		   .resource("http://localhost:8080/RESTfulExample/rest/json/metallica/post");

		String input = "{\"singer\":\"Metallica\",\"title\":\"Fade To Black\"}";

		ClientResponse response = webResource.type("application/json")
		   .post(ClientResponse.class, input);

		if (response.getStatus() != 201) {
			throw new RuntimeException("Failed : HTTP error code : "
			     + response.getStatus());
		}

		System.out.println("Output from Server .... \n");
		String output = response.getEntity(String.class);
		System.out.println(output);

	  } catch (Exception e) {

		e.printStackTrace();

	  }

	}
} 
```

输出…

```java
 Output from Server .... 

Track saved : Track [title=Fade To Black, singer=Metallica] 
```

## 下载源代码

Download it – [Jersey-Client-Example.zip](http://web.archive.org/web/20190309050309/http://www.mkyong.com/wp-content/uploads/2011/07/Jersey-Client-Example.zip) (8 KB)

## 参考

1.  [有球衣+杰克逊的 JSON 例子](http://web.archive.org/web/20190309050309/http://www.mkyong.com/webservices/jax-rs/json-example-with-jersey-jackson/)
2.  [新泽西客户端示例](http://web.archive.org/web/20190309050309/http://blogs.oracle.com/enterprisetechtips/entry/consuming_restful_web_services_with)
3.  [采用 RESTEasy 客户端框架的 RESTful Java 客户端](http://web.archive.org/web/20190309050309/http://www.mkyong.com/webservices/jax-rs/restful-java-client-with-resteasy-client-framework/)
4.  [使用 java.net.URL 的 RESTful Java 客户端](http://web.archive.org/web/20190309050309/http://www.mkyong.com/webservices/jax-rs/restfull-java-client-with-java-net-url/)
5.  [带 Apache HttpClient 的 RESTful Java 客户端](http://web.archive.org/web/20190309050309/http://www.mkyong.com/webservices/jax-rs/restful-java-client-with-apache-httpclient/)

[client](http://web.archive.org/web/20190309050309/http://www.mkyong.com/tag/client/) [jax-rs](http://web.archive.org/web/20190309050309/http://www.mkyong.com/tag/jax-rs/) [jersey](http://web.archive.org/web/20190309050309/http://www.mkyong.com/tag/jersey/) [restful](http://web.archive.org/web/20190309050309/http://www.mkyong.com/tag/restful/)







