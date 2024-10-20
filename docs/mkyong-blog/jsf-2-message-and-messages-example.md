# JSF 新协议消息和消息示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-message-and-messages-example/>

在 JSF，您可以通过以下两个消息标签输出消息:

1.  **h:消息**–为特定组件输出一条消息。
2.  **h:消息**–输出当前页面的所有消息。

参见下面的 JSF 2.0 示例，展示如何使用“ **h:message** ”和“ **h:messages** ”标签来显示验证错误消息。

*JSF 页……*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      >

    <h:body>

    	<h1>JSF 2 message + messages example</h1>

	<h:form>

	  <h:messages style="color:red;margin:8px;" />

	  <br />

	  <h:panelGrid columns="3">

		Enter your username :

		<h:inputText id="username" value="#{user.username}" 
			size="20" required="true"
			label="UserName" >
			<f:validateLength minimum="5" maximum="10" />
		</h:inputText>

		<h:message for="username" style="color:red" />

		Enter your age :
		<h:inputText id="age" value="#{user.age}" 
			size="20" required="true"
			label="Age" >
			<f:validateLongRange for="age" minimum="1" maximum="200" />
		</h:inputText>

		<h:message for="age" style="color:red" />

	  </h:panelGrid>

	  <h:commandButton value="Submit" action="result" />

      </h:form>

    </h:body>
</html> 
```

*这是输出…*



![jsf2-message-example](img/ab8417af990f0afb94ce9e13548e035f.png "jsf2-message-example")

## 下载源代码

Download It – [JSF-2-Message-Example.zip](http://web.archive.org/web/20210122095614/http://www.mkyong.com/wp-content/uploads/2010/11/JSF-2-Message-Example.zip) (10KB)

## 参考

1.  JSF h:消息 JavaDoc
2.  JSF h:消息 JavaDoc

Tags : [jsf2](http://web.archive.org/web/20210122095614/https://mkyong.com/tag/jsf2/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="7616">

### 相关文章

*   [JSF 2.0 教程](/web/20210122095614/https://www.mkyong.com/tutorials/jsf-2-0-tutorials/)
*   [JSF 2 预渲染事件示例](/web/20210122095614/https://www.mkyong.com/jsf2/jsf-2-prerenderviewevent-example/)
*   [JSF 2.0 中的多组件验证器](/web/20210122095614/https://www.mkyong.com/jsf2/multi-components-validator-in-jsf-2-0/)
*   [JSF 2 多选列表框示例](/web/20210122095614/https://www.mkyong.com/jsf2/jsf-2-multiple-select-listbox-example/)
*   [JSF 2 链接、命令链接和输出链接示例](/web/20210122095614/https://www.mkyong.com/jsf2/jsf-2-link-commandlink-and-outputlink-example/)

*   [JSF 2 列表框示例](/web/20210122095614/https://www.mkyong.com/jsf2/jsf-2-listbox-example/)
*   [JSF 2 数据表示例](/web/20210122095614/https://www.mkyong.com/jsf2/jsf-2-datatable-example/)
*   [JSF 2 单选按钮示例](/web/20210122095614/https://www.mkyong.com/jsf2/jsf-2-radio-buttons-example/)
*   [警告:JSF1063:警告！设置不可序列化](/web/20210122095614/https://www.mkyong.com/jsf2/warning-jsf1063-warning-setting-non-serializable-attribute-value-into-httpsession/)
*   [JSF 2 下拉框示例](/web/20210122095614/https://www.mkyong.com/jsf2/jsf-2-dropdown-box-example/)