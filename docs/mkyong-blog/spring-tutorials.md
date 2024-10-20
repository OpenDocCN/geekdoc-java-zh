# 春季教程

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/tutorials/spring-tutorials/>

![Spring tutorials](img/86d8760d2bcdcea01e52923b380bf5a4.png "Spring tutorials")

Rod Johnson 创建的 [Spring framework](http://web.archive.org/web/20220829131356/http://www.springsource.org/) 是一个非常强大的控制反转(IoC)框架，可以帮助分离项目组件的依赖关系。

在这一系列教程中，它提供了许多关于使用 Spring 框架的分步示例和解释。

**New Spring 3.0 Tutorials (23/06/2011)**
Added many Spring 3.0 tutorials on using Spring EL, JavaConfig, AspectJ and Spring Object/XML mapping (oxm). For what’s new in Spring 3.0, you can refer to this [official Spring 3.0 references](http://web.archive.org/web/20220829131356/http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/new-in-3.html).

## 弹簧快速启动

快速了解 Spring 框架开发的基础。

*   [Spring hello world 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/quick-start-maven-spring-example/)
    Maven+Spring 2 . 5 . 6 hello world 示例。
*   [Spring 3.0 hello world 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-3-hello-world-example/)**(Spring 3.0)**
    Maven+Spring 3.0 hello world 示例，新 Spring 3.0 开发需要什么。
*   [Spring 松耦合示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-loosely-coupled-example/)
    一个展示演示 Spring 如何使组件松耦合的例子。

## Spring JavaConfig (Spring 3.0)

Spring 3.0 支持 JavaConfig，现在您可以使用注释在 Spring 中进行配置。

*   [Spring 3 JavaConfig 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-3-javaconfig-example/)
    演示了在 Spring 中使用@Configuration 和@Bean 来定义 Bean
*   [Spring 3 JavaConfig @Import 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-3-javaconfig-import-example/)
    演示了如何使用@Import 来组织模块化的 beans。

## 弹簧依赖注入(DI)

Spring 如何做依赖注入(DI)来管理对象依赖？

*   [Spring 依赖注入(DI)](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-dependency-injection-di/)
    Spring 如何通过 Setter 注入和 Constructor 注入来应用依赖注入(DI)设计模式。
*   [Spring DI via setter 方法](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-di-via-setter-method/)
    依赖注入一个 bean via setter 方法。
*   [Spring DI via 构造函数](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-di-via-constructor/)
    依赖注入 bean via 构造函数。
*   Spring 中的构造函数注入类型不明确
    构造函数注入参数类型不明确的问题总是发生在包含多个具有许多参数的构造函数方法的 bean 中。

## 基本豆

您需要在 Spring Ioc 容器中使用的所有类都被认为是“bean ”,并在 Spring bean 配置文件中或通过注释进行声明。

*   [Spring bean 引用示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-bean-reference-example/)
    bean 如何通过在相同或不同的 bean 配置文件中指定 bean 引用来相互访问。
*   [春天给豆子属性注入价值](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/how-to-define-bean-properties-in-spring/)
    给豆子属性注入价值的三种方式。
*   [加载多个 Spring bean 配置文件](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/load-multiple-spring-bean-configuration-file/)
    开发人员总是将不同的 bean 配置文件归类到不同的模块文件夹中，这里有一个提示告诉你如何加载多个 Spring bean 配置文件。
*   [Spring inner bean 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-inner-bean-examples/)
    每当一个 bean 仅用于一个特定的属性时，总是建议将其声明为一个 inner bean。
*   [Spring bean scopes 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-bean-scopes-examples/)
    bean scopes 用于决定哪种类型的 Bean 实例应该从 Spring 容器返回给调用者。
*   [Spring 集合(列表、集合、映射和属性)示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-collections-list-set-map-and-properties-example/)
    将值注入集合类型(列表、集合、映射和属性)的示例。
*   [ListFactoryBean 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-listfactorybean-example/)
    创建一个具体的列表集合类(ArrayList 和 LinkedList)，并注入到 Bean 属性中。
*   [SetFactoryBean 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-setfactorybean-example/)
    创建一个具体的集合类(HashSet 和 TreeSet)，并注入到 Bean 属性中。
*   [MapFactoryBean 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-mapfactorybean-example/)
    创建一个具体的地图集合类(HashMap 和 TreeMap)，并注入到 Bean 属性中。
*   [Spring 将日期注入 bean 属性–CustomDateEditor](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-how-to-pass-a-date-into-bean-property-customdateeditor/)
    通常，Spring 接受日期变量，这里有一个使用 custom Date editor 来解决它的技巧。
*   [Spring PropertyPlaceholderConfigurer 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-propertyplaceholderconfigurer-example/)
    将部署细节外化到一个属性文件中，并通过一种特殊的格式—＄{ variable }从 bean 配置文件中访问。
*   [Spring bean 配置继承]( http://www.mkyong.com/spring/spring-bean-configuration-inheritance/)
    继承对于 bean 共享公共值、属性或配置非常有用。
*   Spring 依赖检查
    Spring 提供了 4 种依赖检查模式来确保 bean 中已经设置了所需的属性。
*   [Spring 依赖检查带@Required 批注](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-dependency-checking-with-required-annotation/)
    依赖检查在批注模式下。
*   [自定义@Required-style 批注](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-define-custom-required-style-annotation/)
    创建一个自定义@Required-style 批注，相当于@Required 批注。
*   [Bean InitializingBean 和 DisposableBean 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-initializingbean-and-disposablebean-example/)
    在 Bean 初始化和销毁时执行某些操作。(界面)
*   [Bean 初始化方法和销毁方法示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-init-method-and-destroy-method-example/)
    在 Bean 初始化和销毁时执行某些操作。(XML)
*   [Bean @PostConstruct 和@PreDestroy example](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-postconstruct-and-predestroy-example/)
    在 Bean 初始化和销毁时执行某些操作。(注释)

## Spring 表达式语言(Spring 3.0)

Spring 3.0 引入了功能丰富而强大的表达式语言，称为 Spring expression language，或 Spring EL。

*   [Spring EL hello world 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-el-hello-world-example/)
    快速开始使用 Spring 表达式语言(EL)。
*   [春埃尔比恩引用示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-el-bean-reference-example/)
    引用比恩，比恩属性使用点号(。)符号。
*   [Spring EL 方法调用示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-el-method-invocation-example/)
    直接调用 bean 方法。
*   [Spring EL 操作符示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-el-operators-example/)
    Spring EL 支持大多数标准的关系、逻辑和数学操作符。
*   [Spring EL 三元运算符(if-then-else)示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-el-ternary-operator-if-then-else-example/)
    条件检查，if else then。
*   [Spring EL 数组、列表、地图示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-el-lists-maps-example/)
    与地图和列表一起工作。
*   [Spring EL 正则表达式示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-el-regular-expression-example/)
    正则表达式评估条件。
*   [用表达式测试弹簧 EL parser](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/test-spring-el-with-expressionparser/)
    向你展示如何轻松测试弹簧 EL。

## Spring 自动组件扫描

Spring 能够自动扫描、检测和注册您的 bean。

*   [Spring 自动扫描组件](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-auto-scanning-components/)
    使 Spring 能够自动扫描、检测和注册您的 beans。
*   [自动扫描中的弹簧过滤组件](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-filtering-components-in-auto-scanning/)
    示例在自动扫描模式下过滤某些组件。

## 弹簧自动布线 Bean

Spring 的“自动连接”模式可以在 XML 和注释中自动连接或连接 beans。

*   [春季自动布线豆](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-auto-wiring-beans-in-xml/)
    春季 5 种自动布线模式总结。
*   [Spring 按类型自动连接](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-autowiring-by-type/)
    如果一个 bean 的数据类型与其他 bean 属性的数据类型兼容，则自动连接它。
*   [Spring 按名称自动连接](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-autowiring-by-name/)
    如果一个 bean 的名称与另一个 bean 属性的名称相同，则自动连接它。
*   [Spring 由构造函数自动连接](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-autowiring-by-constructor/)
    实际上，它是由构造函数参数中的类型自动连接。
*   [Spring auto wiring by auto detect](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-autowiring-by-autodetect/)
    如果找到默认的构造函数，则选择“autowire by constructor”，否则使用“autowire by type”。
*   [Spring auto wired with @ auto wired annotation](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-auto-wiring-beans-with-autowired-annotation/)
    举例说明如何在注释中定义“自动布线”模式。
*   [Spring auto wiring @ Qualifier Example](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-autowiring-qualifier-example/)
    确定哪个 bean 有资格在字段上自动连线的示例。

## Spring AOP(面向方面编程)

Spring AOP 模块化了方面中的横切关注点。简单来说，一个拦截器来拦截一些方法。

*   [Spring AOP 示例–建议](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-aop-examples-advice/)
    关于不同类型 Spring 建议的示例和说明。
*   [Spring AOP 示例–切入点、顾问](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-aop-example-pointcut-advisor/)
    关于 Spring 不同类型的切入点和顾问的示例和解释。
*   [Spring AOP 拦截器序列](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-aop-interceptor-transaction-is-not-working/)
    AOP 拦截器的序列会影响功能性。
*   这是一个自动代理创建器的例子，可以为你的 beans 自动创建代理对象，有助于避免创建许多重复的代理对象。

## Spring AOP + AspectJ 框架

AspectJ 从 Spring 2.0 开始支持，更加灵活和强大。然而，这个例子是在 Spring 3.0 中演示的。

*   [Spring AOP + AspectJ 注释示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-aop-aspectj-annotation-example/)**(Spring 3.0)**
    一个示例向您展示如何将 AspectJ 注释与 Spring 框架集成。
*   [Spring AOP + AspectJ in XML 配置示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-aop-aspectj-in-xml-configuration-example/)**(Spring 3.0)**
    Spring AOP with AspectJ in XML base 配置。

## Spring 对象/XML 映射器(Spring 3.0)

在 Spring 3.0 中，对象到 XML 映射(OXM)从 Spring Web 服务转移到了核心的 Spring 框架。

*   [Spring Object/XML 映射示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/spring-objectxml-mapping-example/)
    Spring oxm + castor，将对象转换为 XML，反之亦然。

## 弹簧 JDBC 支架

Spring 提供了许多助手类来简化整个 JDBC 数据库操作。

*   [Spring + JDBC 的例子](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/maven-spring-jdbc-example/)
    一个展示如何整合 Spring 和 JDBC 的例子。
*   [JdbcTemplate + JdbcDaoSupport 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-jdbctemplate-jdbcdaosupport-examples/)
    示例使用 Spring 的 JdbcTemplate 和 JdbcDaoSupport 类来简化整个 JDBC 数据库操作过程。
*   [JdbcTemplate 查询示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-jdbctemplate-querying-examples/)
    下面是几个示例，展示如何使用 JdbcTemplate query()方法从数据库中查询或提取数据。
*   [JdbcTemplate batchUpdate()示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-jdbctemplate-batchupdate-example/)
    一个 batchUpdate()示例展示了如何执行批量插入操作。
*   [SimpleJdbcTemplate 查询示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-simplejdbctemplate-querying-examples/)
    以更加用户友好和简单的方式从数据库中查询或提取数据。
*   [SimpleJdbcTemplate batch update()示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-simplejdbctemplate-batchupdate-example/)
    另一个使用 SimpleJdbcTemplate 的批量更新示例，simple JdbcTemplate 是对 JDBC template 的 java5 友好的补充。
*   [SimpleJdbcTemplate 中的命名参数示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-named-parameters-examples-in-simplejdbctemplate/)
    一个示例展示了如何使用命名参数作为 SQL 参数值，而这仅在 simple JDBC template 中受支持。

## 弹簧休眠支持

Spring 附带了许多方便的类来支持 Hibernate ORM 框架。

*   [Maven+Spring+Hibernate+MySql 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/maven-spring-hibernate-mysql-example/)
    一个使用 Spring 和 Hibernate 的简单项目。
*   [Maven+(Spring+Hibernate)Annotation+MySql 示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/maven-spring-hibernate-annotation-mysql-example/)
    一个使用 Spring 和 Hibernate 的简单项目(Annotation 版)。
*   [Spring AOP 在 Hibernate 中的事务管理](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-aop-transaction-management-in-hibernate/)
    一个例子展示了如何用 Spring AOP 管理 Hibernate 事务。
*   [Struts + Spring + Hibernate 集成](http://web.archive.org/web/20220829131356/http://www.mkyong.com/struts/struts-spring-hibernate-integration-example/)
    用 Struts 和 Hibernate 框架集成 Spring 的例子。

## Spring 电子邮件支持

Spring 提供了 MailSender 来通过 JavaMail API 发送电子邮件。

*   [通过 MailSender 发送电子邮件](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-sending-e-mail-via-gmail-smtp-server-with-mailsender/)
    示例使用 Spring 的 MailSender 通过 Gmail SMTP 服务器发送电子邮件。
*   [bean 配置文件中的电子邮件模板](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-define-an-e-mail-template-in-bean-configuration-file/)
    在方法体中硬编码所有的电子邮件属性和消息内容并不是一个好的做法，你应该考虑在 Spring 的 bean 配置文件中定义电子邮件消息模板。
*   [发送带有附件的电子邮件](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-sending-e-mail-with-attachment/)
    示例使用 Spring 发送带有附件的电子邮件。

## 春季调度支持

Spring 在 JDK 计时器和石英框架中都有很好的支持。

*   [Spring + JDK 定时器调度器示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-jdk-timer-scheduler-example/)
    一篇关于 Spring 如何用 JDK 定时器调度作业的文章。
*   [Spring + Quartz 调度器示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-quartz-scheduler-example/)
    一篇关于 Spring 如何用 Quartz 框架调度作业的文章。
*   [Spring + Struts + Quartz 调度器示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/struts/struts-spring-quartz-scheduler-integration-example/)
    集成 Spring 和 Struts，用 Quartz 框架调度作业。

## 将 Spring 与其他 Web 框架集成

Spring 集成了其他 web 框架。

*   [servlet 会话监听器中的 Spring 依赖注入](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-how-to-do-dependency-injection-in-your-session-listener/)
    Spring 附带了一个“ContextLoaderListener”监听器，作为在会话监听器和几乎所有其他 web 框架中启用 Spring 依赖注入的通用方式。
*   [Struts + Spring 集成](http://web.archive.org/web/20220829131356/http://www.mkyong.com/struts/struts-spring-integration-example/)
    用 Struts 1.x 框架集成 Spring 的例子。
*   [Struts 2 + Spring 集成示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/struts2/struts-2-spring-integration-example/)
    将 Spring 与 Struts 2 框架集成的示例。
*   [JSF 2.0 + Spring 集成示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/jsf2/jsf-2-0-spring-integration-example/)
    将 JSF 2.0 与 Spring 框架集成的示例。
*   [JSF 2.0 + Spring + Hibernate 集成示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/jsf2/jsf-2-0-spring-hibernate-integration-example/)
    将 JSF 2.0 + Spring + Hibernate 框架集成在一起的示例。
*   [Wicket + Spring 集成示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/wicket/wicket-spring-integration-example/)
    Wicket 与 Spring 框架集成的示例。
*   [Struts 2 + Spring + Quartz 调度器集成示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/struts2/struts-2-spring-quartz-scheduler-integration-example/)
    集成 Spring + Struts 2 + Quartz 的示例。
*   [Struts 2 + Spring + Hibernate 集成示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/struts2/struts-2-spring-hibernate-integration-example/)
    集成 Spring + Struts 2 + Hibernate 的示例。

## 春季常见问题

*   [在 Eclipse 中安装 Spring IDE](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/how-to-install-spring-ide-in-eclipse/)
    一篇关于如何在 Eclipse 中安装 Spring IDE 的文章。
*   [带有 ResourceBundleMessageSource 的资源包示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-resource-bundle-with-resourcebundlemessagesource-example/)
    ResourceBundleMessageSource 是为不同地区解析文本消息的最常见的类。
*   [在 bean 中访问 MessageSource(MessageSourceAware)]( http://www.mkyong.com/spring/spring-how-to-access-messagesource-in-bean-messagesourceaware/)
    这个例子展示了如何通过 MessageSourceAware 接口在 bean 中获取 message source。
*   [带有 getResource()的资源加载器示例](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-resource-loader-with-getresource-example/)
    Spring 的资源加载器提供了一个非常通用的 getResource()方法来从文件系统、类路径或 URL 中获取资源，比如(文本文件、媒体文件、图像文件……)。

## Spring 常见错误

一些 Spring 常见的错误消息。

*   [ClassNotFoundException:org . spring framework . web . context . context loader listener](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-error-classnotfoundexception-org-springframework-web-context-contextloaderlistener/)
*   [无法代理目标类，因为 CGLIB2 不可用](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring/spring-aop-error-cannot-proxy-target-class-because-cglib2-is-not-available/)
*   [需要 CGLIB 来处理@Configuration 类](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/cglib-is-required-to-process-configuration-classes/)
*   [Java . lang . classnotfoundexception:org . exolab . castor . XML . XML exception](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/classnotfoundexception-org-exolab-castor-xml-xmlexception/)
*   [Java . lang . classnotfoundexception:org . Apache . XML . serialize . XML serializer](http://web.archive.org/web/20220829131356/http://www.mkyong.com/spring3/classnotfoundexception-org-apache-xml-serialize-xmlserializer/)

## 弹簧参考

*   [Spring 框架(Wiki)](http://web.archive.org/web/20220829131356/https://en.wikipedia.org/wiki/Spring_Framework)
*   [春季官方文件](http://web.archive.org/web/20220829131356/http://www.springsource.org/documentation)
*   [Spring 2.5.6 文档](http://web.archive.org/web/20220829131356/http://static.springsource.org/spring/docs/2.5.x/reference/index.html)
*   [Spring 3.0 文档](http://web.archive.org/web/20220829131356/http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/)

<input type="hidden" id="mkyong-current-postId" value="4293">