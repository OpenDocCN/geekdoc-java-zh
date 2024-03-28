# 管理引荐控件

> 原文：[`docs.oracle.com/javase/tutorial/jndi/newstuff/mdsaIT.html`](https://docs.oracle.com/javase/tutorial/jndi/newstuff/mdsaIT.html)

管理引荐控件（[RFC 3296](http://www.ietf.org/rfc/rfc3296.txt)）允许在执行 LDAP 操作时将引荐和其他特殊对象操作为普通对象。换句话说，管理引荐控件告诉 LDAP 服务器将引荐条目返回为普通条目，而不是返回"引荐"错误响应或继续引用。 JDK 5.0 中的新类使您可以在 LDAP 请求中发送管理引荐控件：

[`javax.naming.ldap.ManageReferralControl`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ldap/ManageReferralControl.html)

JDK 中的 LDAP 服务提供程序将自动发送此控件以及任何请求。您还可以通过将 Context.REFERRAL 环境属性设置为"ignore"来显式启用它。有关引荐处理的更多信息，请查看 JNDI 教程的[引荐](https://docs.oracle.com/javase/jndi/tutorial/ldap/referral/jndi.html)部分。

这是一个示例，它在 LDAP 请求中发送了管理引荐控件。

```java
         // Create initial context
         LdapContext ctx = (LdapContext) new InitialDirContext(env);
         ctx.setRequestControl(new Control[] new ManageReferralControl());

         // Set controls for performing subtree search
         SearchControls ctls = new SearchControls();
         ctls.setSearchScope(SearchControls.SUBTREE_SCOPE);

         // Perform search
         NamingEnumeration answer = ctx.search("", "(objectclass=*)", ctls);

         // Print the answer
         while (answer.hasMore()) {
             System.out.println(">>>" + 
                 ((SearchResult)answer.next()).getName());
          }

          // Close the context when we're done
          ctx.close();

```

完整示例可以在`这里`找到。

* * *

**注意 1：** 以上示例将要求您使用配置文件`refserver.ldif`设置第二个服务器。服务器必须支持 LDAP v3 和 RFC 3296。如果服务器不支持这种方式的引荐，则示例将无法正常工作。配置文件包含引荐，指向您设置的原始服务器。它假定原始服务器位于本地机器的端口 389 上。如果您在另一台机器或端口上设置了服务器，则需要编辑 refserver.ldif 文件中的"ref"条目，并将"localhost:389"替换为适当的设置。第二个服务器应在本地机器的端口 489 上设置。如果您在另一台机器或端口上设置了第二个服务器，则需要相应地调整初始上下文的 Context.PROVIDER_URL 环境属性的设置。

设置目录服务器通常由目录或系统管理员执行。有关更多信息，请参阅软件设置课程。

**注意 2：** Windows Active Directory：由于 Active Directory 不支持管理引荐控件，本课程中的所有示例都无法针对 Active Directory 工作。

* * *
