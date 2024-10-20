# 带有 Apache HttpClient 的 RESTful Java 客户端

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/webservices/jax-rs/restful-java-client-with-apache-httpclient/>

Apache HttpClient 是一个健壮且完整的解决方案 Java 库，用于执行 HTTP 操作，包括 RESTful 服务。在本教程中，我们将向您展示如何使用 **Apache HttpClient** 创建一个 RESTful Java 客户端，来执行一个“ **GET** 和“ **POST** 请求。

**Note**
The RESTful services from last “[Jackson + JAX-RS](http://web.archive.org/web/20221117052655/http://www.mkyong.com/webservices/jax-rs/integrate-jackson-with-resteasy/)” article will be reused.

## 1.获取 Apache HttpClient

Apache HttpClient 在 Maven 中央存储库中可用，只需在 Maven `pom.xml`文件中声明它。

*文件:pom.xml*

```java
 <dependency>
		<groupId>org.apache.httpcomponents</groupId>
		<artifactId>httpclient</artifactId>
		<version>4.1.1</version>
	</dependency> 
```

## 2.获取请求

再次回顾上次休息服务。

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

Apache HttpClient 发送一个“GET”请求。

```java
 import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.DefaultHttpClient;

public class ApacheHttpClientGet {

	public static void main(String[] args) {
	  try {

		DefaultHttpClient httpClient = new DefaultHttpClient();
		HttpGet getRequest = new HttpGet(
			"http://localhost:8080/RESTfulExample/json/product/get");
		getRequest.addHeader("accept", "application/json");

		HttpResponse response = httpClient.execute(getRequest);

		if (response.getStatusLine().getStatusCode() != 200) {
			throw new RuntimeException("Failed : HTTP error code : "
			   + response.getStatusLine().getStatusCode());
		}

		BufferedReader br = new BufferedReader(
                         new InputStreamReader((response.getEntity().getContent())));

		String output;
		System.out.println("Output from Server .... \n");
		while ((output = br.readLine()) != null) {
			System.out.println(output);
		}

		httpClient.getConnectionManager().shutdown();

	  } catch (ClientProtocolException e) {

		e.printStackTrace();

	  } catch (IOException e) {

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

Apache HttpClient 发送一个“POST”请求。

```java
 import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.MalformedURLException;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.DefaultHttpClient;

public class ApacheHttpClientPost {

	public static void main(String[] args) {

	  try {

		DefaultHttpClient httpClient = new DefaultHttpClient();
		HttpPost postRequest = new HttpPost(
			"http://localhost:8080/RESTfulExample/json/product/post");

		StringEntity input = new StringEntity("{\"qty\":100,\"name\":\"iPad 4\"}");
		input.setContentType("application/json");
		postRequest.setEntity(input);

		HttpResponse response = httpClient.execute(postRequest);

		if (response.getStatusLine().getStatusCode() != 201) {
			throw new RuntimeException("Failed : HTTP error code : "
				+ response.getStatusLine().getStatusCode());
		}

		BufferedReader br = new BufferedReader(
                        new InputStreamReader((response.getEntity().getContent())));

		String output;
		System.out.println("Output from Server .... \n");
		while ((output = br.readLine()) != null) {
			System.out.println(output);
		}

		httpClient.getConnectionManager().shutdown();

	  } catch (MalformedURLException e) {

		e.printStackTrace();

	  } catch (IOException e) {

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

Download it – [RESTful-Java-Client-ApacheHttpClient-Example.zip](http://web.archive.org/web/20221117052655/http://www.mkyong.com/wp-content/uploads/2011/07/RESTful-Java-Client-ApacheHttpClient-Example.zip) (9 KB)

## 参考

1.  [杰克逊官网](http://web.archive.org/web/20221117052655/http://jackson.codehaus.org/ )
2.  [Apache HttpClient](http://web.archive.org/web/20221117052655/https://hc.apache.org/httpcomponents-client-ga/index.html)
3.  [使用 java.net.URL 的 RESTful Java 客户端](http://web.archive.org/web/20221117052655/http://www.mkyong.com/webservices/jax-rs/restfull-java-client-with-java-net-url/)

<input type="hidden" id="mkyong-current-postId" value="9624">