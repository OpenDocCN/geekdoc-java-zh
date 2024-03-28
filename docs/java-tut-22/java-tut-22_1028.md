# 排序控制

> 原文：[`docs.oracle.com/javase/tutorial/jndi/newstuff/sort.html`](https://docs.oracle.com/javase/tutorial/jndi/newstuff/sort.html)

当客户端希望服务器发送排序后的搜索结果时，使用排序控制。服务器端排序在客户端需要根据某些标准对结果进行排序但无法自行执行排序过程的情况下非常有用。排序控制在[RFC 2891](http://www.ietf.org/rfc/rfc2891.txt)中指定。下面的类提供了支持排序控制所需的功能。

+   [`javax.naming.ldap.SortControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortControl.html)

+   [`javax.naming.ldap.SortKey`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortKey.html)

+   [`javax.naming.ldap.SortResponseControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/SortResponseControl.html)

SortKey 是一个基于其排序结果的键的有序列表。

## 如何使用排序控制？

下面的示例说明了客户端执行搜索请求服务器端基于属性“cn”进行排序的客户端-服务器交互。

1.  客户端发送搜索请求请求

    ```java

        // Activate sorting
         String sortKey = "cn";
         ctx.setRequestControls(new Control[] { 
             new SortControl(sortKey, Control.CRITICAL) });

         // Perform a search
         NamingEnumeration results = 
             ctx.search("", "(objectclass=*)", new SearchControls());

    ```

1.  服务器响应的条目是根据“cn”属性及其对应的匹配规则排序的。

    ```java

        // Iterate over sorted search results
         while (results != null && results.hasMore()) {
             // Display an entry
             SearchResult entry = (SearchResult)results.next();
             System.out.println(entry.getName());

             // Handle the entry's response controls (if any)
             if (entry instanceof HasControls) {
                 // ((HasControls)entry).getControls();
             }
         }
         // Examine the sort control response 
         Control[] controls = ctx.getResponseControls();
         if (controls != null) {
             for (int i = 0; i < controls.length; i++) {
                 if (controls[i] instanceof SortResponseControl) {
                     SortResponseControl src = (SortResponseControl)controls[i];
                     if (! src.isSorted()) {
                         throw src.getException();
                     }
                 } else {
                     // Handle other response controls (if any)
                 }
             }
         }  

    ```

完整的 JNDI 示例可以在`这里`找到。

* * *

**注意：** 排序控制由 Oracle Directory Server 和 Windows Active Directory 服务器都支持。

* * *
