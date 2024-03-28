# LDAP 错误代码与 JNDI 异常的映射方式

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ldap/exceptions.html`](https://docs.oracle.com/javase/tutorial/jndi/ldap/exceptions.html)

LDAP 定义了一组状态代码，这些代码是 LDAP 服务器发送的 LDAP 响应中返回的（参见[RFC 2251](http://www.ietf.org/rfc/rfc2251.txt)）。在 JNDI 中，错误条件被指示为[`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html)的子类的已检查异常。请参阅 Naming Exceptions 部分以获取 JNDI 异常类的概述。

LDAP 服务提供程序将从 LDAP 服务器接收的 LDAP 状态代码转换为适当的`NamingException`子类。以下表格显示了 LDAP 状态代码与 JNDI 异常之间的映射。

| LDAP 状态代码 | 含义 | 异常或操作 |
| --- | --- | --- |
| 0 | 成功 | 报告成功。 |
| 1 | 操作错误 | [`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html) |
| 2 | 协议错误 | [`CommunicationException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/CommunicationException.html) |
| 3 | 超出时间限制。 | [`TimeLimitExceededException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/TimeLimitExceededException.html) |
| 4 | 大小限制超出。 | [`SizeLimitExceededException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/SizeLimitExceededException.html) |
| 5 | 比较为假。 | 由[`DirContext.search()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-javax.naming.directory.Attributes-)使用。不会生成异常。 |
| 6 | 比较为真。 | 由[`DirContext.search()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-javax.naming.directory.Attributes-)使用。不会生成异常。 |
| 7 | 不支持的身份验证方法。 | [`AuthenticationNotSupportedException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/AuthenticationNotSupportedException.html) |
| 8 | 需要强身份验证。 | [`AuthenticationNotSupportedException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/AuthenticationNotSupportedException.html) |
| 9 | 正在返回部分结果。 | 如果环境属性`"java.naming.referral"`设置为`"ignore"`或错误内容不包含引荐，则抛出[`PartialResultException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/PartialResultException.html)。否则，使用内容构建引荐。 |
| 10 | 遇到引荐。 | 如果环境属性`"java.naming.referral"`设置为`"ignore"`，则忽略。如果属性设置为`"throw"`，则抛出[`ReferralException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ReferralException.html)。如果属性设置为`"follow"`，则 LDAP 提供程序处理引荐。如果已超过`"java.naming.ldap.referral.limit"`属性，则抛出[`LimitExceededException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/LimitExceededException.html)。 |
| 11 | 超出管理限制。 | [`LimitExceededException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/LimitExceededException.html) |
| 12 | 请求的不可用关键扩展。 | [`OperationNotSupportedException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/OperationNotSupportedException.html) |
| 13 | 需要机密性。 | [`AuthenticationNotSupportedException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/AuthenticationNotSupportedException.html) |
| 14 | SASL 绑定正在进行中。 | 用于 LDAP 提供程序在认证过程中的内部使用。 |
| 16 | 不存在此属性。 | [`NoSuchAttributeException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/NoSuchAttributeException.html) |
| 17 | 未定义的属性类型。 | [`InvalidAttributeIdentifierException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InvalidAttributeIdentifierException.html) |
| 18 | 不匹配。 | [`InvalidSearchFilterException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InvalidSearchFilterException.html) |
| 19 | 约束违规。 | [`InvalidAttributeValueException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InvalidAttributeValueException.html) |
| 20 | 属性或值已在使用中。 | [`AttributeInUseException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/AttributeInUseException.html) |
| 21 | 无效的属性语法。 | [`InvalidAttributeValueException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/InvalidAttributeValueException.html) |
| 32 | 不存在此对象。 | [`NameNotFoundException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameNotFoundException.html) |
| 33 | 别名问题。 | [`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html) |
| 34 | 无效的 DN 语法。 | [`InvalidNameException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/InvalidNameException.html) |
| 35 | 是一个叶子。 | 由 LDAP 提供程序使用；通常不会生成异常。 |
| 36 | 别名解引用问题。 | [`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html) |
| 48 | 不适当的身份验证。 | [`AuthenticationNotSupportedException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/AuthenticationNotSupportedException.html) |
| 49 | 无效凭证 | [`AuthenticationException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/AuthenticationException.html) |
| 50 | 访问权限不足 | [`NoPermissionException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NoPermissionException.html) |
| 51 | 忙碌 | [`ServiceUnavailableException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ServiceUnavailableException.html) |
| 52 | 不可用 | [`ServiceUnavailableException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ServiceUnavailableException.html) |
| 53 | 不愿执行 | [`OperationNotSupportedException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/OperationNotSupportedException.html) |
| 54 | 检测到循环。 | [`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html) |
| 64 | 命名违规 | [`InvalidNameException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/InvalidNameException.html) |
| 65 | 对象类违规 | [`SchemaViolationException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SchemaViolationException.html) |
| 66 | 非叶子节点不允许。 | [`ContextNotEmptyException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/ContextNotEmptyException.html) |
| 67 | RDN 上不允许。 | [`SchemaViolationException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SchemaViolationException.html) |
| 68 | 条目已存在。 | [`NameAlreadyBoundException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NameAlreadyBoundException.html) |
| 69 | 禁止对象类修改。 | [`SchemaViolationException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SchemaViolationException.html) |
| 71 | 影响多个 DSA。 | [`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html) |
| 80 | 其他 | [`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html) |
