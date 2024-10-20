# 在 Java 中将 InputStream 转换为 BufferedReader

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/convert-inputstream-to-bufferedreader-in-java/>

**注**
[缓冲阅读器](http://web.archive.org/web/20220629160041/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/BufferedReader.html)读取字符；而[输入流](http://web.archive.org/web/20220629160041/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InputStream.html)是字节流。

`BufferedReader`不能直接读取`InputStream`；所以，我们需要使用像 [InputStreamReader](http://web.archive.org/web/20220629160041/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InputStreamReader.html) 这样的适配器将字节转换成字符格式。例如:

```java
 // BufferedReader -> InputStreamReader -> InputStream
  BufferedReader br = new BufferedReader(
                          new InputStreamReader(inputStream, StandardCharsets.UTF_8)); 
```

## 1。从资源文件夹中读取文件。

这个例子从 resources 文件夹中读取一个文件作为`InputStream`；而且我们可以用`BufferedReader + InputStreamReader`逐行读取。

InputStreamToReaderExample.java

```java
 package com.mkyong.io.howto;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;

public class InputStreamToReaderExample {

    public static void main(String[] args) throws IOException {

        // loads a file from a resources folder
        InputStream is = InputStreamToReaderExample.class
                            .getClassLoader()
                            .getResourceAsStream("file/abc.txt");

        // BufferedReader -> InputStreamReader -> InputStream

        // try-with-resources, auto close
        String line;
        try (BufferedReader br = new BufferedReader(
                      new InputStreamReader(is, StandardCharsets.UTF_8))) {

            // read line by line
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }

        }

    }
} 
```

## 2。向 Cloudflare 端点发送 Http post 请求。

这个示例使用 [Apache HttpClient](/web/20220629160041/https://mkyong.com/java/apache-httpclient-examples/) 向 Cloudflare 端点发送 POST 请求，以阻止一个 IP 地址。在`InputStream`中返回的是一个 JSON 字符串，我们可以使用同一个`BufferedReader + InputStreamReader`来读取字节流。

pom.xml

```java
 <dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.13</version>
  </dependency> 
```

CloudFlareBanIP.java

```java
 package com.mkyong.security.action;

import com.mkyong.security.util.PropertyUtils;
import org.apache.http.Header;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.message.BasicHeader;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.concurrent.TimeUnit;

public class CloudFlareBanIP {

    private static final String CF_AUTH_EMAIL = PropertyUtils.getInstance().getValue("cf_auth_email");
    private static final String CF_AUTH_TOKEN = PropertyUtils.getInstance().getValue("cf_auth_token");
    private static final String JSON_TYPE = "application/json";
    private static Header[] HTTP_HEADERS = {
            new BasicHeader("X-Auth-Email", CF_AUTH_EMAIL),
            new BasicHeader("X-Auth-Key", CF_AUTH_TOKEN),
            new BasicHeader("content-type", JSON_TYPE)
    };

    private HttpClient httpClient = HttpClientBuilder.create()
            .setConnectionTimeToLive(10, TimeUnit.SECONDS)
            .build();

    public HttpClient getHttpClient() {
        return httpClient;
    }

    public static void main(String[] args) throws IOException {

        CloudFlareBanIP obj = new CloudFlareBanIP();
        String response = obj.banIp("52.249.189.81", "bad ip");

        System.out.println(response);

    }

    public String banIp(String ip, String note) throws IOException {

        StringBuilder json = new StringBuilder();
        json.append("{");
        json.append("\"mode\":\"block\",");
        json.append("\"configuration\":" + "{\"target\":\"ip\",\"value\":\"" + ip + "\"}" + ",");
        json.append("\"notes\":\"" + note + "\"");
        json.append("}");

        StringBuilder result = new StringBuilder();

        HttpPost post = new HttpPost("https://api.cloudflare.com/client/v4/user/firewall/access_rules/rules");
        post.setHeaders(HTTP_HEADERS);
        post.setEntity(new StringEntity(json.toString()));

        // read response from the POST request
        try (BufferedReader br = new BufferedReader(
                new InputStreamReader(getHttpClient().execute(post).getEntity().getContent()))) {
            String line;
            while ((line = br.readLine()) != null) {
                result.append(line);
            }
        }

        return result.toString();

    }

} 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629160041/https://github.com/mkyong/core-java)

$ CD Java-io/操作方法

## 参考文献

*   [BufferedReader JavaDoc](http://web.archive.org/web/20220629160041/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/BufferedReader.html)
*   [InputStream JavaDoc](http://web.archive.org/web/20220629160041/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InputStream.html)
*   [InputStreamReader Java](http://web.archive.org/web/20220629160041/https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/InputStreamReader.html)
*   [InputStream 转字符串示例](/web/20220629160041/https://mkyong.com/java/how-to-convert-inputstream-to-string-in-java/)

<input type="hidden" id="mkyong-current-postId" value="16426">