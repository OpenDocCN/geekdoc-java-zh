# 分页结果控件

> 原文：[`docs.oracle.com/javase/tutorial/jndi/newstuff/paged-results.html`](https://docs.oracle.com/javase/tutorial/jndi/newstuff/paged-results.html)

## BasicControl

[`javax.naming.ldap.BasicControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/BasicControl.html) 实现了 `javax.naming.ldap.Control`，作为扩展其他控件的基本实现。

## 分页结果控件

分页结果控件对于希望以受控方式接收搜索结果的 LDAP 客户端非常有用，受页面大小限制。页面大小可以由客户端根据其资源的可用性（如带宽和处理能力）进行配置。

服务器使用 cookie（类似于 HTTP 会话 cookie 机制）来维护搜索请求的状态，以跟踪发送给客户端的结果。分页结果控件在 [RFC 2696](http://www.ietf.org/rfc/rfc2696.txt) 中指定。下面的类提供了支持分页结果控件所需的功能。

+   [`javax.naming.ldap.PagedResultsControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/PagedResultsControl.html)

+   [`javax.naming.ldap.PagedResultsResponseControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/PagedResultsResponseControl.html)

### 如何使用分页结果控件？

下面的示例说明了客户端执行搜索并请求页面大小限制为 5 的客户端-服务器交互。服务器返回的整个结果集包含 21 个条目。

1.  客户端发送搜索请求，请求页面大小为 5 的分页结果。

    ```java
        // Activate paged results
         int pageSize = 5; // 5 entries per page
         byte[] cookie = null;
         int total;
         ctx.setRequestControls(new Control[]{ 
             new PagedResultsControl(pageSize, Control.CRITICAL) });
         // Perform the search
         NamingEnumeration results = ctx.search("", "(objectclass=*)", 
                                                new SearchControls());

    ```

1.  服务器响应包含条目以及搜索结果中总共 21 个条目的指示，还有一个不透明的 cookie，客户端在检索后续页面时要使用该 cookie。

    ```java
       // Iterate over a batch of search results sent by the server
             while (results != null && results.hasMore()) {
                 // Display an entry
                 SearchResult entry = (SearchResult)results.next();
                 System.out.println(entry.getName());

                 // Handle the entry's response controls (if any)
                 if (entry instanceof HasControls) {
                     // ((HasControls)entry).getControls();
                 }
             }
       // Examine the paged results control response 
             Control[] controls = ctx.getResponseControls();
             if (controls != null) {
                 for (int i = 0; i < controls.length; i++) {
                     if (controls[i] instanceof PagedResultsResponseControl) {
                         PagedResultsResponseControl prrc =
                             (PagedResultsResponseControl)controls[i];
                         total = prrc.getResultSize();
                         cookie = prrc.getCookie();
                     } else {
                         // Handle other response controls (if any)
                     }
                 }
             }   

    ```

1.  客户端发送相同的搜索请求，返回不透明的 cookie，并请求下一页。

    ```java
      // Re-activate paged results
             ctx.setRequestControls(new Control[]{
                 new PagedResultsControl(pageSize, cookie, Control.CRITICAL) });

    ```

1.  服务器响应包含五个条目，并指示还有更多条目。客户端重复执行第 4 步中执行的搜索，直到服务器返回空 cookie，表示服务器不再发送更多条目。

完整的 JNDI 示例可以在 `这里` 找到。

* * *

**注意：** 分页搜索控件受 Windows Active Directory 服务器支持。Oracle Directory Server 版本 5.2 不支持。

* * *
