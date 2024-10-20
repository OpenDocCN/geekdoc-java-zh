# 如何在 Wicket 中加密/编码 URL

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/how-do-encrypt-encode-url-in-wicket/>

在 Wicket 中，编码或加密 URL 是一件非常容易的事情，这个特性是默认提供的，你只需要激活它。

## Wicket 默认普通 URL

```java
 http://localhost:8080/WicketExamples/?wicket:interface=:0:urlQueryPanel: 
```

## Wicket 编码或加密的 URL

```java
 http://localhost:8080/WicketExamples/?x=YwbAGQPpvT9MHF2-6S6FwvocqYPuA 
```

要启用这个特性，请将下面的代码粘贴到 Wicket 的 web 应用程序类中。

```java
 @Override
	protected IRequestCycleProcessor newRequestCycleProcessor() {

		return new WebRequestCycleProcessor() {
			protected IRequestCodingStrategy newRequestCodingStrategy() {
				return new CryptedUrlWebRequestCodingStrategy(
					new WebRequestCodingStrategy());
			}
		};
	} 
```

查看完整示例…

```java
 package com.mkyong;

import org.apache.wicket.protocol.http.WebApplication;
import org.apache.wicket.protocol.http.WebRequestCycleProcessor;
import org.apache.wicket.protocol.http.request.CryptedUrlWebRequestCodingStrategy;
import org.apache.wicket.protocol.http.request.WebRequestCodingStrategy;
import org.apache.wicket.request.IRequestCodingStrategy;
import org.apache.wicket.request.IRequestCycleProcessor;
import com.mkyong.user.UserPage;

public class WicketApplication extends WebApplication {

	@Override
	public Class<UserPage> getHomePage() {

		return UserPage.class; // return default page
	}

	@Override
	protected IRequestCycleProcessor newRequestCycleProcessor() {

		return new WebRequestCycleProcessor() {
			protected IRequestCodingStrategy newRequestCodingStrategy() {
				return new CryptedUrlWebRequestCodingStrategy(
					new WebRequestCodingStrategy());
			}
		};
	}

} 
```

完成了。

## 一点想法

Wicket 使用基于密码的加密机制来编码和解码 URL。所有必需的类都位于"**wicket-1.4-rc1-sources . jar \ org \ Apache \ wicket \ util \ crypt**"中。我认为最强大的功能是个人会话随机加密密钥，它使用会话和 UUID 来生成它，以便每个访问者使用自己的加密密钥(不同的 http 会话)。

*文件:KeyInSessionSunJceCryptFactory.java*

```java
 if (key == null)
{
	// generate new key
	key = session.getId() + "." + UUID.randomUUID().toString();
	session.setAttribute(keyAttr, key);
} 
```

在基于密码的加密机制中,“Salt”和“Iterator count”是公开的，但是使用如上所述的强加密密钥(会话+ UUID ),这使得 wicket 的 URL 编码函数非常难以解码，即使您手头有 Wicket 的源代码。

如果你认为 Wicket 的默认加密机制不够安全，你可以很容易地实现不同的加密机制，如 **AES** 或 **SHA** 。

**Note**
Wicket’s is using SunJceCrypt class as default URL encode and decode implementation.

## 参考

1.  [Wicket URL 编码策略](http://web.archive.org/web/20201109034909/https://cwiki.apache.org/WICKET/url-coding-strategies.html)
2.  [Wicket 混淆网址](http://web.archive.org/web/20201109034909/https://cwiki.apache.org/WICKET/obfuscating-urls.html)

Tags : [url](http://web.archive.org/web/20201109034909/https://mkyong.com/tag/url/) [wicket](http://web.archive.org/web/20201109034909/https://mkyong.com/tag/wicket/)<input type="hidden" id="mkyong-current-postId" value="1068">

### 相关文章

*   [如何在 ja 中编码 URL 字符串或表单参数](/web/20201109034909/https://mkyong.com/java/how-to-encode-a-url-string-or-form-parameter-in-java/)
*   [如何在 Java 中验证 URL](/web/20201109034909/https://mkyong.com/java/how-to-validate-url-in-java/)
*   [如何在 Java 中获取 URL 内容](/web/20201109034909/https://mkyong.com/java/how-to-get-url-content-in-java/)
*   [如何在 Eclipse 中设置 Wicket 示例](/web/20201109034909/https://mkyong.com/wicket/how-do-setup-wicket-examples-in-eclipse/)
*   [Java . lang . classnotfoundexception:org . slf4j . impl .](/web/20201109034909/https://mkyong.com/wicket/java-lang-classnotfoundexception-org-slf4j-impl-staticloggerbinder/)

*   [Wicket 文本框示例](/web/20201109034909/https://mkyong.com/wicket/wicket-textbox-example/)
*   [Wicket 属性模型示例](/web/20201109034909/https://mkyong.com/wicket/wicket-propertymodel-example/)
*   [Wicket Hello World 示例](/web/20201109034909/https://mkyong.com/wicket/wicket-hello-world-example-with-maven-tutorial/)
*   [Wicket + Log4j 集成示例](/web/20201109034909/https://mkyong.com/wicket/wicket-log4j-integration-example/)
*   [启用 org.apache.wicket.util.r 的调试消息](/web/20201109034909/https://mkyong.com/wicket/enable-debug-messages-for-org-apache-wicket-util-resource/)