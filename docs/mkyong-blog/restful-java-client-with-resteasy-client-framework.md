# 带有 RESTEasy 客户端框架的 RESTful Java 客户端

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/webservices/jax-rs/restful-java-client-with-resteasy-client-framework/>

本教程向您展示了如何使用 RESTEasy 客户端框架创建一个 RESTful Java 客户端，来执行在上一个教程中创建的“ **GET** ”和“**POST**”REST 服务请求。

## 1.RESTEasy 客户端框架

RESTEasy 客户端框架包含在 RESTEasy 核心模块中，所以，你只需要在你的`pom.xml`文件中声明“ **resteasy-jaxrs.jar** ”。

*文件:pom.xml*

```java
 <dependency>
		<groupId>org.jboss.resteasy</groupId>
		<artifactId>resteasy-jaxrs</artifactId>
		<version>2.2.1.GA</version>
	</dependency> 
```

 ## 2.获取请求

回顾上次休息服务。

```java
 @Path("/json/product")
public class JSONService {

	@GET
	@Path("/get")
	@Produces("application/json")
	public Product getProductInJSON() {

		Product product = new Product();
		product.setName("iPad 3");
		product.setQty(999);

		return product; 

	}
	//... 
```

RESTEasy 客户端发送一个“GET”请求。

```java
 import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import org.apache.http.client.ClientProtocolException;
import org.jboss.resteasy.client.ClientRequest;
import org.jboss.resteasy.client.ClientResponse;

public class RESTEasyClientGet {

	public static void main(String[] args) {
	  try {

		ClientRequest request = new ClientRequest(
				"http://localhost:8080/RESTfulExample/json/product/get");
		request.accept("application/json");
		ClientResponse<String> response = request.get(String.class);

		if (response.getStatus() != 200) {
			throw new RuntimeException("Failed : HTTP error code : "
				+ response.getStatus());
		}

		BufferedReader br = new BufferedReader(new InputStreamReader(
			new ByteArrayInputStream(response.getEntity().getBytes())));

		String output;
		System.out.println("Output from Server .... \n");
		while ((output = br.readLine()) != null) {
			System.out.println(output);
		}

	  } catch (ClientProtocolException e) {

		e.printStackTrace();

	  } catch (IOException e) {

		e.printStackTrace();

	  } catch (Exception e) {

		e.printStackTrace();

	  }

	}

} 
```

输出…

```java
 Output from Server .... 

{"qty":999,"name":"iPad 3"} 
```

 ## 3.发布请求

也回顾最后一次休息服务。

```java
 @Path("/json/product")
public class JSONService {

        @POST
	@Path("/post")
	@Consumes("application/json")
	public Response createProductInJSON(Product product) {

		String result = "Product created : " + product;
		return Response.status(201).entity(result).build();

	}
	//... 
```

RESTEasy 客户端发送一个“POST”请求。

```java
 import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.MalformedURLException;
import org.jboss.resteasy.client.ClientRequest;
import org.jboss.resteasy.client.ClientResponse;

public class RESTEasyClientPost {

	public static void main(String[] args) {

	  try {

		ClientRequest request = new ClientRequest(
			"http://localhost:8080/RESTfulExample/json/product/post");
		request.accept("application/json");

		String input = "{\"qty\":100,\"name\":\"iPad 4\"}";
		request.body("application/json", input);

		ClientResponse<String> response = request.post(String.class);

		if (response.getStatus() != 201) {
			throw new RuntimeException("Failed : HTTP error code : "
				+ response.getStatus());
		}

		BufferedReader br = new BufferedReader(new InputStreamReader(
			new ByteArrayInputStream(response.getEntity().getBytes())));

		String output;
		System.out.println("Output from Server .... \n");
		while ((output = br.readLine()) != null) {
			System.out.println(output);
		}

	  } catch (MalformedURLException e) {

		e.printStackTrace();

	  } catch (IOException e) {

		e.printStackTrace();

	  } catch (Exception e) {

		e.printStackTrace();

	  }

	}

} 
```

输出…

```java
 Output from Server .... 

Product created : Product [name=iPad 4, qty=100] 
```

## 下载源代码

Download it – [RESTful-Java-Client-RESTEasyt-Example.zip](http://web.archive.org/web/20190214234129/http://www.mkyong.com/wp-content/uploads/2011/07/RESTful-Java-Client-RESTEasyt-Example.zip) (9 KB)

## 参考

1.  [杰克逊官网](http://web.archive.org/web/20190214234129/http://jackson.codehaus.org/ )
2.  [使用 java.net.URL 的 RESTful Java 客户端](http://web.archive.org/web/20190214234129/http://www.mkyong.com/webservices/jax-rs/restfull-java-client-with-java-net-url/)
3.  [带 Apache HttpClient 的 RESTful Java 客户端](http://web.archive.org/web/20190214234129/http://www.mkyong.com/webservices/jax-rs/restful-java-client-with-apache-httpclient/)

[client](http://web.archive.org/web/20190214234129/http://www.mkyong.com/tag/client/) [jax-rs](http://web.archive.org/web/20190214234129/http://www.mkyong.com/tag/jax-rs/) [resteasy](http://web.archive.org/web/20190214234129/http://www.mkyong.com/tag/resteasy/) [restful](http://web.archive.org/web/20190214234129/http://www.mkyong.com/tag/restful/)







