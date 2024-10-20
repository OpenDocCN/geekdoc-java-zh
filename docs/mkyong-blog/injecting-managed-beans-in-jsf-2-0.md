# 在 JSF 2.0 中注入受管 beans

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/injecting-managed-beans-in-jsf-2-0/>

在 JSF 2.0 中，一个新的 **@ManagedProperty** 注释用于将一个受管 bean 依赖注入(DI)到另一个受管 bean 的属性中。

让我们看一个 **@ManagedProperty** 的例子:

**MessageBean.java**——名为“**消息**的托管 bean。

```java
 import java.io.Serializable;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;

@ManagedBean(name="message")
@SessionScoped
public class MessageBean implements Serializable {

	//business logic and whatever methods...

} 
```

**——将**消息**bean 注入到“**消息 Bean** 属性中。**

```java
 `import java.io.Serializable;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.ManagedProperty;
import javax.faces.bean.SessionScoped;

@ManagedBean
@SessionScoped
public class HelloBean implements Serializable {

	@ManagedProperty(value="#{message}")
	private MessageBean messageBean;

	//must povide the setter method
	public void setMessageBean(MessageBean messageBean) {
		this.messageBean = messageBean;
	}

	//...
}` 
```

**在本例中，它使用了 **@ManagedProperty** 注释，通过 setter 方法 **setMessageBean()** ，将“消息”bean(【MessageBean.java】)解析为“你好”bean(**【HelloBean.java】**)的属性( **messageBean** )。**

****Note**
To make this injection successful, the inject property (**messageBean**) must provide the setter method.**

## **下载源代码**

**Download it – [JSF-2-Inject-Managed-Beans-Example.zip](http://web.archive.org/web/20201127022853/http://www.mkyong.com/wp-content/uploads/2010/09/JSF-2-Inject-Managed-Beans-Example.zip) (10KB)**

## **参考**

1.  **[ManagedProperty Javadoc](http://web.archive.org/web/20201127022853/http://download.oracle.com/javaee/6/api/javax/faces/bean/ManagedProperty.html)**
2.  **[JSF 2.0:受管 bean x 不存在，检查适当的 getter 和/或 setter 方法是否存在](http://web.archive.org/web/20201127022853/http://www.mkyong.com/jsf2/jsf-2-0-managed-bean-x-does-not-exist-check-that-appropriate-getter-andor-setter-methods-exist/)**

**Tags : [jsf2](http://web.archive.org/web/20201127022853/https://mkyong.com/tag/jsf2/) [managed bean](http://web.archive.org/web/20201127022853/https://mkyong.com/tag/managed-bean/)****<input type="hidden" id="mkyong-current-postId" value="7046">

### 相关文章

*   [在 JSF 2.0 中配置受管 Beans】](/web/20201127022853/https://www.mkyong.com/jsf2/configure-managed-beans-in-jsf-2-0/)
*   [JSF 2.0:被管理的 bean x 不存在，检查一下](/web/20201127022853/https://www.mkyong.com/jsf2/jsf-2-0-managed-bean-x-does-not-exist-check-that-appropriate-getter-andor-setter-methods-exist/)
*   [JSF 2.0 教程](/web/20201127022853/https://www.mkyong.com/tutorials/jsf-2-0-tutorials/)
*   [JSF 2 预渲染事件示例](/web/20201127022853/https://www.mkyong.com/jsf2/jsf-2-prerenderviewevent-example/)
*   [JSF 2.0 中的多组件验证器](/web/20201127022853/https://www.mkyong.com/jsf2/multi-components-validator-in-jsf-2-0/)

*   [JSF 2 多选列表框示例](/web/20201127022853/https://www.mkyong.com/jsf2/jsf-2-multiple-select-listbox-example/)
*   [JSF 2 链接、命令链接和输出链接示例](/web/20201127022853/https://www.mkyong.com/jsf2/jsf-2-link-commandlink-and-outputlink-example/)
*   [JSF 2 列表框示例](/web/20201127022853/https://www.mkyong.com/jsf2/jsf-2-listbox-example/)
*   [JSF 2 数据表示例](/web/20201127022853/https://www.mkyong.com/jsf2/jsf-2-datatable-example/)
*   [JSF 2 单选按钮示例](/web/20201127022853/https://www.mkyong.com/jsf2/jsf-2-radio-buttons-example/)**