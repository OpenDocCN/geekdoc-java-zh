# 在 JSF 2.0 中配置受管 Beans

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/configure-managed-beans-in-jsf-2-0/>

在 JSF 2.0 中，可以从 JSF 页面访问的 Java bean 被称为**托管 Bean** 。受管 bean 可以是一个普通的 Java bean，它包含 getter 和 setter 方法、业务逻辑，甚至是一个后备 bean(一个 bean 包含所有的 HTML 表单值)。

有两种方法可以配置受管 bean:

## 1.使用注释配置受管 Bean

在 JSF 2.0 中，可以用新的 **@ManagedBean** 注释来注释受管 Bean。

```java
 package com.mkyong.common;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import java.io.Serializable;

@ManagedBean
@SessionScoped
public class HelloBean implements Serializable {

	private static final long serialVersionUID = 1L;

	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
} 
```

## 2.用 XML 配置受管 Bean

使用 XML 配置，您可以使用旧的 JSF 1.x 机制在普通的 **faces-config.xml** 文件中定义受管 bean。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<faces-config

    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
    http://java.sun.com/xml/ns/javaee/web-facesconfig_2_0.xsd"
    version="2.0">
    <managed-bean>
	  <managed-bean-name>helloBean</managed-bean-name>
	  <managed-bean-class>com.mkyong.common.HelloBean</managed-bean-class>
	  <managed-bean-scope>session</managed-bean-scope>
     </managed-bean>
</faces-config> 
```

**Best Practice**
It’s recommended to put the managed beans in a separate XML file because the **faces-config.xml** is used to set the application level configurations.

因此，您应该创建一个新的 XML 文件并将受管 beans 细节放入其中，并在**javax . faces . config _ FILES**initialize 参数中声明该 XML 文件，该参数位于 **WEB-INF/web.xml** 文件中。

**web.xml**

```java
 ...
 <context-param>
    <param-name>javax.faces.CONFIG_FILES</param-name>
    <param-value>WEB-INF/manage-beans.xml</param-value>
  </context-param>
... 
```

## 下载源代码

Download it – [JSF-2-Managed-Beans-Example.zip](http://web.archive.org/web/20201127022850/http://www.mkyong.com/wp-content/uploads/2010/09/JSF-2-Managed-Beans-Example.zip) (10KB)Tags : [jsf2](http://web.archive.org/web/20201127022850/https://mkyong.com/tag/jsf2/) [managed bean](http://web.archive.org/web/20201127022850/https://mkyong.com/tag/managed-bean/)<input type="hidden" id="mkyong-current-postId" value="7026">

### 相关文章

*   [JSF 2.0:被管理的 bean x 不存在，检查一下](/web/20201127022850/https://www.mkyong.com/jsf2/jsf-2-0-managed-bean-x-does-not-exist-check-that-appropriate-getter-andor-setter-methods-exist/)
*   [在 JSF 2.0 中注入托管 beans】](/web/20201127022850/https://www.mkyong.com/jsf2/injecting-managed-beans-in-jsf-2-0/)
*   [JSF 2.0 教程](/web/20201127022850/https://www.mkyong.com/tutorials/jsf-2-0-tutorials/)
*   [JSF 2 预渲染事件示例](/web/20201127022850/https://www.mkyong.com/jsf2/jsf-2-prerenderviewevent-example/)
*   [JSF 2.0 中的多组件验证器](/web/20201127022850/https://www.mkyong.com/jsf2/multi-components-validator-in-jsf-2-0/)

*   [JSF 2 多选列表框示例](/web/20201127022850/https://www.mkyong.com/jsf2/jsf-2-multiple-select-listbox-example/)
*   [JSF 2 链接、命令链接和输出链接示例](/web/20201127022850/https://www.mkyong.com/jsf2/jsf-2-link-commandlink-and-outputlink-example/)
*   [JSF 2 列表框示例](/web/20201127022850/https://www.mkyong.com/jsf2/jsf-2-listbox-example/)
*   [JSF 2 数据表示例](/web/20201127022850/https://www.mkyong.com/jsf2/jsf-2-datatable-example/)
*   [JSF 2 单选按钮示例](/web/20201127022850/https://www.mkyong.com/jsf2/jsf-2-radio-buttons-example/)