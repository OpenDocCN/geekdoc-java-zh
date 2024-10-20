# 配置 Struts 标记库

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/configure-the-struts-tag-libraries/>

在 Struts 框架中，您总是需要配置 Struts 标记库，以便在视图页面(JSP)中访问它。有两种方法可以配置它。

## 1.支柱标记库手动配置

手动配置是古老而经典的方式，在 **Struts 版本< = 1.1 和 Servlet < 2.3** 容器中使用。下载所有的 Struts 依赖项，确保下面的“ **tld** 文件被复制到 **WEB-INF** 文件夹，你可以在下载的 Struts 库中找到这些文件。

*   struts-bean.tld
*   struts-html.tld
*   struts-logic.tld
*   struts-tiles.tld

在 web.xml
**web.xml** 中声明标记库 uri

```java
 ...
<taglib>
     <taglib-uri>
	  http://struts.apache.org/tags-bean
     </taglib-uri>
     <taglib-location>
	  /WEB-INF/struts-bean.tld
     </taglib-location>
</taglib>
<taglib>
     <taglib-uri>
	  http://struts.apache.org/tags-html
     </taglib-uri>
     <taglib-location>
	  /WEB-INF/struts-html.tld
     </taglib-location>
</taglib>
<taglib>
     <taglib-uri>
	  http://struts.apache.org/tags-logic
     </taglib-uri>
     <taglib-location>
	  /WEB-INF/struts-logic.tld
     </taglib-location>
</taglib>
<taglib>
     <taglib-uri>
	  http://struts.apache.org/tags-tiles
     </taglib-uri>
     <taglib-location>
	  /WEB-INF/struts-tiles.tld
     </taglib-location>
</taglib>
<taglib>
     <taglib-uri>
	  http://struts.apache.org/tags-nested
     </taglib-uri>
     <taglib-location>
	  /WEB-INF/struts-nested.tld
     </taglib-location>
</taglib>
... 
```

现在您可以在 JSP 页面中访问它。JSP 的 **@taglib uri** 必须与 web . XML**<taglib-uri>**匹配

```java
<%@ taglib uri="http://struts.apache.org/tags-bean" prefix="bean" %>
<%@ taglib uri="http://struts.apache.org/tags-html" prefix="html" %>
<%@ taglib uri="http://struts.apache.org/tags-logic" prefix="logic" %>
<%@ taglib uri="http://struts.apache.org/tags-tiles" prefix="tiles" %>

```

实际上，您可以定义自己的 **taglib uri** 名称，例如

**web.xml**

```java
 ...
<taglib>
     <taglib-uri>
	  customer-anything/tags-bean
     </taglib-uri>
     <taglib-location>
	  /WEB-INF/struts-bean.tld
     </taglib-location>
</taglib>
... 
```

然后通过您的自定义 **taglib uri** 名称访问它。

```java
<%@ taglib uri="customer-anything/tags-bean" prefix="bean" %>

```

 ## 2.支柱标签库自动配置

这是一种简单的方法，只在 Struts、1.3 和 Servlet 2.3/2.4 容器中使用。您不再需要在 web.xml 中定义“ **tlds** ”细节，只需要在您的项目类路径中包含 **struts-taglib.jar** 或者将其复制到 WEB-INF/lib 文件夹中。

所有的“ **tld** ”细节都在“**struts-taglib . jar \ META-INF \ TLD**”文件夹中定义。部署过程中， **struts-bean.tld** 、 **struts-html.tld** 、 **struts-logic.tld** 和 **struts-tiles.tld** 会自动部署。但是，您只能通过下面的“**预先固定的 uri** ”名称来访问它。在此方法中，不允许您更改“ **taglib uri** ”名称。

```java
<%@ taglib uri="http://struts.apache.org/tags-bean" prefix="bean" %>
<%@ taglib uri="http://struts.apache.org/tags-html" prefix="html" %>
<%@ taglib uri="http://struts.apache.org/tags-logic" prefix="logic" %>
<%@ taglib uri="http://struts.apache.org/tags-tiles" prefix="tiles" %>

```

 ## 常见问题解答

**问:看起来“taglib uri”指向 Apache 网站，客户端无法访问互联网怎么办？**
**A**:taglib uri 定义在“**struts-taglib . jar \ META-INF \ TLD**”文件夹中，它只是一个项目 uri 名称，并不指向 Apache 网站，即使在没有互联网访问的环境下也可以访问。

问:手动配置可以在最新的 Struts 1.2 或 1.3 中工作吗？
**A** :是的，Struts 是向后兼容的，老办法还是在 Struts 1.2 和 1.3 中支持。

**问:哪种方法最好？**
**A** :靠，自动配置只在 Servlet 2.3/2.4 容器中有效。如果让你选择，请转到自动方式，为什么要手动复制 tld 文件？

## 参考

*   [http://struts.apache.org/1.2.8/userGuide/configuration.html](http://web.archive.org/web/20190224170101/http://struts.apache.org/1.2.8/userGuide/configuration.html)

[struts](http://web.archive.org/web/20190224170101/http://www.mkyong.com/tag/struts/)







