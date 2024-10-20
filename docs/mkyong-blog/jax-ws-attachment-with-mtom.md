# JAX-WS 与 MTOM 的关系

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/webservices/jax-ws/jax-ws-attachment-with-mtom/>

一个完整的基于 JAX-WS SOAP 的示例，展示了如何使用**消息传输优化机制(MTOM)** 和**XML-二进制优化打包(XOP)** 技术在服务器和客户端之间发送二进制附件(图像)。

**Note**
There are ton of articles about what’s MTOM and the benefits of using it (see [reference sites](#reference) below), but it’s lack of complete example to use **MTOM in JAX-WS**, hope this example can fill in some missing pieces in the whole picture.

## 在服务器上启用 MTOM

让服务器通过 MTOM 发送附件非常容易，只需用`javax.xml.ws.soap.MTOM`注释 web 服务实现类。

## 1.web 服务端点

这里有一个 RPC 风格的 web 服务，发布了两个方法，`downloadImage(String name)`和`uploadImage(Image data)`，让用户上传或下载图像文件。

*文件:ImageServer.java*

```java
 package com.mkyong.ws;

import java.awt.Image;

import javax.jws.WebMethod;
import javax.jws.WebService;
import javax.jws.soap.SOAPBinding;
import javax.jws.soap.SOAPBinding.Style;

//Service Endpoint Interface
@WebService
@SOAPBinding(style = Style.RPC)
public interface ImageServer{

	//download a image from server
	@WebMethod Image downloadImage(String name);

	//update image to server
	@WebMethod String uploadImage(Image data);

} 
```

*文件:ImageServerImpl.java*

```java
 package com.mkyong.ws;

import java.awt.Image;
import java.io.File;
import java.io.IOException;

import javax.imageio.ImageIO;
import javax.jws.WebService;
import javax.xml.ws.WebServiceException;
import javax.xml.ws.soap.MTOM;

//Service Implementation Bean
@MTOM
@WebService(endpointInterface = "com.mkyong.ws.ImageServer")
public class ImageServerImpl implements ImageServer{

	@Override
	public Image downloadImage(String name) {

		try {

			File image = new File("c:\\images\\" + name);
			return ImageIO.read(image);

		} catch (IOException e) {

			e.printStackTrace();
			return null; 

		}
	}

	@Override
	public String uploadImage(Image data) {

		if(data!=null){
			//store somewhere
			return "Upload Successful";
		}

		throw new WebServiceException("Upload Failed!");

	}

} 
```

*文件:ImagePublisher.java*

```java
 package com.mkyong.endpoint;

import javax.xml.ws.Endpoint;
import com.mkyong.ws.ImageServerImpl;

//Endpoint publisher
public class ImagePublisher{

    public static void main(String[] args) {

	Endpoint.publish("http://localhost:9999/ws/image", new ImageServerImpl());

	System.out.println("Server is published!");

    }

} 
```

## 2.web 服务客户端

这里有一个 web 服务客户端，用于访问位于 URL "*http://localhost:9999/ws/image*的已发布的 web 服务。

*文件:ImageClient.java*

```java
 package com.mkyong.client;

import java.awt.Image;
import java.io.File;
import java.net.URL;
import javax.imageio.ImageIO;
import javax.swing.ImageIcon;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.xml.namespace.QName;
import javax.xml.ws.BindingProvider;
import javax.xml.ws.Service;
import javax.xml.ws.soap.MTOMFeature;
import javax.xml.ws.soap.SOAPBinding;

import com.mkyong.ws.ImageServer;

public class ImageClient{

	public static void main(String[] args) throws Exception {

	URL url = new URL("http://localhost:9999/ws/image?wsdl");
        QName qname = new QName("http://ws.mkyong.com/", "ImageServerImplService");

        Service service = Service.create(url, qname);
        ImageServer imageServer = service.getPort(ImageServer.class);

        /************  test download  ***************/
        Image image = imageServer.downloadImage("rss.png");

        //display it in frame
        JFrame frame = new JFrame();
        frame.setSize(300, 300);
        JLabel label = new JLabel(new ImageIcon(image));
        frame.add(label);
        frame.setVisible(true);

        System.out.println("imageServer.downloadImage() : Download Successful!");

    }

} 
```

## 3.HTTP 和 SOAP 流量

这是客户端生成的 HTTP 和 SOAP 流量，由流量监控工具捕获。

为了节省空间，省略了第一个 wsdl 请求。
**客户端发送请求:**

```java
 POST /ws/image HTTP/1.1
SOAPAction: ""
Accept: text/xml, multipart/related, text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2
Content-Type: text/xml; charset=utf-8
User-Agent: Java/1.6.0_13
Host: localhost:9999
Connection: keep-alive
Content-Length: 209

<?xml version="1.0" ?>
	<S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
		<S:Body>
			<ns2:downloadImage xmlns:ns2="http://ws.mkyong.com/">
				<arg0>rss.png</arg0>
			</ns2:downloadImage>
		</S:Body>
	</S:Envelope> 
```

**服务器发送响应:**

```java
 HTTP/1.1 200 OK
Transfer-encoding: chunked
Content-type: multipart/related;
start="<rootpart*c73c9ce8-6e02-40ce-9f68-064e18843428@example.jaxws.sun.com>";
type="application/xop+xml";
boundary="uuid:c73c9ce8-6e02-40ce-9f68-064e18843428";
start-info="text/xml"

--uuid:c73c9ce8-6e02-40ce-9f68-064e18843428
Content-Id: <rootpart*c73c9ce8-6e02-40ce-9f68-064e18843428@example.jaxws.sun.com>
Content-Type: application/xop+xml;charset=utf-8;type="text/xml"
Content-Transfer-Encoding: binary

<?xml version="1.0" ?>
  <S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
     <S:Body>
	<ns2:downloadImageResponse xmlns:ns2="http://ws.mkyong.com/">
	  <return>
	    <xop:Include xmlns:xop="http://www.w3.org/2004/08/xop/include" 
		href="cid:012eb00e-9460-407c-b622-1be987fdb2cf@example.jaxws.sun.com">
	    </xop:Include>
	  </return>
	</ns2:downloadImageResponse>
     </S:Body>
   </S:Envelope>
--uuid:c73c9ce8-6e02-40ce-9f68-064e18843428
Content-Id: <012eb00e-9460-407c-b622-1be987fdb2cf@example.jaxws.sun.com>
Content-Type: image/png
Content-Transfer-Encoding: binary

Binary data here............. 
```

## 在客户端启用 MTOM

允许客户端通过 MTOM 向服务器发送附件需要一些额外的工作，请参见以下示例:

```java
 //codes enable MTOM in client
BindingProvider bp = (BindingProvider) imageServer;
SOAPBinding binding = (SOAPBinding) bp.getBinding();
binding.setMTOMEnabled(true); 
```

## 1.web 服务客户端

下面是一个 webservice 客户端，它通过 MTOM 将一个图像文件发送到上面发布的端点(http://localhost:8888/ws/image)。

*文件:ImageClient.java*

```java
 package com.mkyong.client;

import java.awt.Image;
import java.io.File;
import java.net.URL;
import javax.imageio.ImageIO;
import javax.swing.ImageIcon;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.xml.namespace.QName;
import javax.xml.ws.BindingProvider;
import javax.xml.ws.Service;
import javax.xml.ws.soap.MTOMFeature;
import javax.xml.ws.soap.SOAPBinding;

import com.mkyong.ws.ImageServer;

public class ImageClient{

	public static void main(String[] args) throws Exception {

	URL url = new URL("http://localhost:8888/ws/image?wsdl");
        QName qname = new QName("http://ws.mkyong.com/", "ImageServerImplService");

        Service service = Service.create(url, qname);
        ImageServer imageServer = service.getPort(ImageServer.class);

        /************  test upload ****************/
        Image imgUpload = ImageIO.read(new File("c:\\images\\rss.png"));

        //enable MTOM in client
        BindingProvider bp = (BindingProvider) imageServer;
        SOAPBinding binding = (SOAPBinding) bp.getBinding();
        binding.setMTOMEnabled(true);

        String status = imageServer.uploadImage(imgUpload);
        System.out.println("imageServer.uploadImage() : " + status);

    }

} 
```

## 2.HTTP 和 SOAP 流量

这是客户端生成的 HTTP 和 SOAP 流量，由流量监控工具捕获。

为了节省空间，省略了第一个 wsdl 请求。
**客户端发送请求:**

```java
 POST /ws/image HTTP/1.1
Content-type: multipart/related;
start="<rootpart*751f2e5d-47f8-47d8-baf0-f793c29bd931@example.jaxws.sun.com>";
type="application/xop+xml";
boundary="uuid:751f2e5d-47f8-47d8-baf0-f793c29bd931";

start-info="text/xml"
Soapaction: ""
Accept: text/xml, multipart/related, text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2
User-Agent: JAX-WS RI 2.1.6 in JDK 6
Host: localhost:9999
Connection: keep-alive
Content-Length: 6016

--uuid:751f2e5d-47f8-47d8-baf0-f793c29bd931
Content-Id: <rootpart*751f2e5d-47f8-47d8-baf0-f793c29bd931@example.jaxws.sun.com>
Content-Type: application/xop+xml;charset=utf-8;type="text/xml"
Content-Transfer-Encoding: binary

<?xml version="1.0" ?>
  <S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
     <S:Body>
	<ns2:uploadImage xmlns:ns2="http://ws.mkyong.com/">
	  <arg0>
	    <xop:Include xmlns:xop="http://www.w3.org/2004/08/xop/include" 
		href="cid:2806f201-e15e-4ee0-8347-b7b4dffad5cb@example.jaxws.sun.com">
	    </xop:Include>
	  </arg0>
	 </ns2:uploadImage>
     </S:Body>
   </S:Envelope>

--uuid:751f2e5d-47f8-47d8-baf0-f793c29bd931
Content-Id: <2806f201-e15e-4ee0-8347-b7b4dffad5cb@example.jaxws.sun.com>
Content-Type: image/png
Content-Transfer-Encoding: binary

Binary data here............. 
```

**服务器发送响应:**

```java
 HTTP/1.1 200 OK
Transfer-encoding: chunked
Content-type: multipart/related;
start="<rootpart*188a5835-198b-4c28-9b36-bf030578f2bd@example.jaxws.sun.com>";
type="application/xop+xml";
boundary="uuid:188a5835-198b-4c28-9b36-bf030578f2bd";
start-info="text/xml"

--uuid:188a5835-198b-4c28-9b36-bf030578f2bd
Content-Id: <rootpart*188a5835-198b-4c28-9b36-bf030578f2bd@example.jaxws.sun.com>
Content-Type: application/xop+xml;charset=utf-8;type="text/xml"
Content-Transfer-Encoding: binary

<?xml version="1.0" ?>
  <S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
      <S:Body>
	<ns2:uploadImageResponse xmlns:ns2="http://ws.mkyong.com/">
		<return>Upload Successful</return>
	</ns2:uploadImageResponse>
      </S:Body>
  </S:Envelope>

--uuid:188a5835-198b-4c28-9b36-bf030578f2bd-- 
```

## 完整的 WSDL 文档

有兴趣研究 WSDL 文件的，可以通过 URL:*http://localhost:9999/ws/image 获取 wsdl 文件？wsdl*

*ImageServer WSDL 文件示例*

```java
 <?xml version="1.0" encoding="UTF-8"?>

<definitions 
	xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/" 
	xmlns:tns="http://ws.mkyong.com/" 
	xmlns:xsd="http://www.w3.org/2001/XMLSchema" 

	targetNamespace="http://ws.mkyong.com/" 
	name="ImageServerImplService">

<types></types>

<message name="downloadImage">
	<part name="arg0" type="xsd:string"></part>
</message>
<message name="downloadImageResponse">
	<part name="return" type="xsd:base64Binary"></part>
</message>

<message name="uploadImage">
	<part name="arg0" type="xsd:base64Binary"></part>
</message>
<message name="uploadImageResponse">
	<part name="return" type="xsd:string"></part>
</message>

<portType name="ImageServer">

	<operation name="downloadImage">
		<input message="tns:downloadImage"></input>
		<output message="tns:downloadImageResponse"></output>
	</operation>
	<operation name="uploadImage">
		<input message="tns:uploadImage"></input>
		<output message="tns:uploadImageResponse"></output>
	</operation>

</portType>

<binding name="ImageServerImplPortBinding" type="tns:ImageServer">
	<soap:binding transport="http://schemas.xmlsoap.org/soap/http" style="rpc">
        </soap:binding>

	<operation name="downloadImage">
		<soap:operation soapAction=""></soap:operation>
		<input>
			<soap:body use="literal" namespace="http://ws.mkyong.com/"></soap:body>
		</input>
		<output>
			<soap:body use="literal" namespace="http://ws.mkyong.com/"></soap:body>
		</output>
	</operation>

	<operation name="uploadImage">
		<soap:operation soapAction=""></soap:operation>
		<input>
			<soap:body use="literal" namespace="http://ws.mkyong.com/">
                        </soap:body>
		</input>
		<output>
			<soap:body use="literal" namespace="http://ws.mkyong.com/">
                        </soap:body>
		</output>
	</operation>

</binding>

<service name="ImageServerImplService">
<port name="ImageServerImplPort" binding="tns:ImageServerImplPortBinding">
<soap:address location="http://localhost:9999/ws/image"></soap:address>
</port>
</service>
</definitions> 
```

## 下载源代码

Download It – [JAX-WS-Attachment-MTOM-Example.zip](http://web.archive.org/web/20210610050633/http://www.mkyong.com/wp-content/uploads/2010/11/JAX-WS-Attachment-MTOM-Example.zip) (20KB)

## 参考

1.  [http://www.devx.com/xml/Article/34797/1763/page/1](http://web.archive.org/web/20210610050633/http://www.devx.com/xml/Article/34797/1763/page/1)
2.  [http://download . Oracle . com/docs/CD/e 17802 _ 01/web services/web services/docs/2.0/jaxws/mtom-swaref . html](http://web.archive.org/web/20210610050633/https://download.oracle.com/docs/cd/E17802_01/webservices/webservices/docs/2.0/jaxws/mtom-swaref.html)
3.  [http://www.crosschecknet.com/intro_to_mtom.php](http://web.archive.org/web/20210610050633/http://www.crosschecknet.com/intro_to_mtom.php)
4.  [http://download . Oracle . com/docs/CD/e 12840 _ 01/WLS/docs 103/webserv _ adv/mtom . html](http://web.archive.org/web/20210610050633/https://download.oracle.com/docs/cd/E12840_01/wls/docs103/webserv_adv/mtom.html)
5.  [http://www . the server side . com/news/1363957/Sending-Attachments-with-SOAP](http://web.archive.org/web/20210610050633/http://www.theserverside.com/news/1363957/Sending-Attachments-with-SOAP)
6.  [http://metro.java.net/guide/Binary_Attachments__MTOM_.html](http://web.archive.org/web/20210610050633/http://metro.java.net/guide/Binary_Attachments__MTOM_.html)

Tags : [attachment](http://web.archive.org/web/20210610050633/https://mkyong.com/tag/attachment/) [jax-ws](http://web.archive.org/web/20210610050633/https://mkyong.com/tag/jax-ws/) [mtom](http://web.archive.org/web/20210610050633/https://mkyong.com/tag/mtom/) [web services](http://web.archive.org/web/20210610050633/https://mkyong.com/tag/web-services/)<input type="hidden" id="mkyong-current-postId" value="7764">