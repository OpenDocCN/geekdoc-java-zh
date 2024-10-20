# 如何显示 hibernate sql 参数值–P6Spy

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/how-to-display-hibernate-sql-parameter-values-solution/>

## 问题

有很多开发人员询问关于 Hibernate SQL 参数值的问题。如何显示传递给数据库的 Hibernate SQL 参数值？Hibernate 只是将所有参数值显示为问号(？).利用 [show_sql](http://web.archive.org/web/20210109151106/http://www.mkyong.com/hibernate/hibernate-display-generated-sql-to-console-show_sql-format_sql-and-use_sql_comments/) 属性，Hibernate 将显示所有生成的 sql 语句，但不显示 SQL 参数值。

*例如*

```java
 Hibernate: insert into mkyong.stock_transaction (CHANGE, CLOSE, DATE, OPEN, STOCK_ID, VOLUME) 
values (?, ?, ?, ?, ?, ?) 
```

有办法记录或显示准确的 Hibernate SQL 参数值吗？

## 解决方案–P6Spy

嗯，有问题就有答案~

P6Spy 是一个有用的库，可以在将所有 SQL 语句和参数值发送到数据库之前记录它们。P6Spy 是免费的，它用于拦截和记录所有数据库 SQL 语句到一个日志文件中，它适用于任何使用 JDBC 驱动程序的应用程序。

## 1.下载 P6Spy 库

获取“ **p6spy-install.jar** ”，您可以从

1.  [P6Spy 官网](http://web.archive.org/web/20210109151106/http://www.p6spy.com/)。
2.  在 Sourceforge.net 的间谍活动

## 2.提取它

提取 p6spy-install.jar 文件，查找 **p6spy.jar** 和 **spy.properties**

## 3.添加库依赖项

将 **p6spy.jar** 添加到您的项目库依赖项中

## 4.修改 P6Spy 属性文件

修改您的数据库配置文件。您需要将您现有的 JDBC 驱动程序替换为 P6Spy JDBC 驱动程序-“T0”

*原文是 MySQL JDBC 驱动——“com . MySQL . JDBC . driver”*

```java
 <session-factory>
  <property name="hibernate.bytecode.use_reflection_optimizer">false</property>
  <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
  <property name="hibernate.connection.password">password</property>
  <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/mkyong</property>
  <property name="hibernate.connection.username">root</property>
  <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
  <property name="show_sql">true</property>
</session-factory> 
```

*改为 P6Spy JDBC 驱动——“com . P6Spy . engine . spy . p6spydriver”*

```java
 <session-factory>
  <property name="hibernate.bytecode.use_reflection_optimizer">false</property>
  <property name="hibernate.connection.driver_class">com.p6spy.engine.spy.P6SpyDriver
  </property>
  <property name="hibernate.connection.password">password</property>
  <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/mkyong</property>
  <property name="hibernate.connection.username">root</property>
  <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
  <property name="show_sql">true</property>
</session-factory> 
```

## 5.修改 P6Spy 属性文件

修改 P6Spy 属性文件-"**spy . properties**"

用现有的 MySQL JDBC 驱动程序替换“真正的驱动程序”

```java
 realdriver=com.mysql.jdbc.Driver

#specifies another driver to use
realdriver2=
#specifies a third driver to use
realdriver3= 
```

**更改日志文件位置**
在 **logfile** 属性中更改日志文件位置，所有 SQL 语句都会记录到这个文件中。

**窗户**

```java
 logfile     = c:/spy.log 
```

***nix**

```java
 logfile     = /srv/log/spy.log 
```

## 6.将“spy.properties”复制到项目类路径中

将“spy.properties”复制到您的项目根文件夹中，确保您的项目可以找到“spy.properties”，否则将提示“spy.properties”文件未找到异常。

## 7.完成的

运行您的应用程序并执行一些数据库事务，您会注意到从应用程序发送到数据库的所有 SQL 语句都将被记录到您在“spy.properties”中指定的文件中。

示例日志文件如下。

```java
 insert into mkyong.stock_transaction (CHANGE, CLOSE, DATE, OPEN, STOCK_ID, VOLUME) 
values (?, ?, ?, ?, ?, ?)|
insert into mkyong.stock_transaction (CHANGE, CLOSE, DATE, OPEN, STOCK_ID, VOLUME) 
values (10.0, 1.1, '2009-12-30', 1.2, 11, 1000000) 
```

## 结论

坦率地说，P6Spy 在减少开发人员的调试时间方面非常有用。只要您的项目使用 JDBC 驱动程序进行连接，P6Sqp 就可以进入它并为您记录所有 SQL 语句和参数值。

## 对于 Maven 用户

您可以使用 Maven 将 P6Spy 依赖项下载到您的`pom.xml`中

```java
 <dependency>
		<groupId>p6spy</groupId>
		<artifactId>p6spy</artifactId>
		<version>1.3</version>
	</dependency> 
```

然而,“spy.properties”文件不在包中，你必须自己创建它。你可以在这里下载模板-[spy . properties](http://web.archive.org/web/20210109151106/http://www.mkyong.com/wp-content/uploads/2008/12/spy.properties.zip)

## 参考

1.  [P6Spy 配置](http://web.archive.org/web/20210109151106/http://www.p6spy.com/documentation/install.htm)

Tags : [hibernate](http://web.archive.org/web/20210109151106/https://mkyong.com/tag/hibernate/) [parameter](http://web.archive.org/web/20210109151106/https://mkyong.com/tag/parameter/)<input type="hidden" id="mkyong-current-postId" value="544">

### 相关文章

*   [如何显示 hibernate sql 参数值- Lo](/web/20210109151106/https://mkyong.com/hibernate/how-to-display-hibernate-sql-parameter-values-log4j/)
*   [Java . lang . classnotfoundexception:org . object web . as](/web/20210109151106/https://mkyong.com/java/java-lang-classnotfoundexception-org-objectweb-asm-type/)
*   为什么我的项目选择 Hibernate？
*   [记住序数参数是从 1 开始的！-嗨](/web/20210109151106/https://mkyong.com/hibernate/remember-that-ordinal-parameters-are-1-based-hibernatetemplate/)
*   [如何在 Hibernate - Logback 中配置日志记录](/web/20210109151106/https://mkyong.com/hibernate/how-to-configure-logging-in-hibernate-logback/)

*   [org . hibernate . annotation 异常:未知 Id.gene](/web/20210109151106/https://mkyong.com/hibernate/org-hibernate-annotationexception-unknown-id-generator/)
*   [如何在 Eclipse 中安装 Hibernate / JBoss 工具](/web/20210109151106/https://mkyong.com/hibernate/how-to-install-hibernate-tools-in-eclipse-ide/)
*   [如何生成 Hibernate 映射文件& annotati](/web/20210109151106/https://mkyong.com/hibernate/how-to-generate-code-with-hibernate-tools/)
*   [Maven 3+Hibernate 3.6+Oracle 11g 示例(XML](/web/20210109151106/https://mkyong.com/hibernate/maven-3-hibernate-3-6-oracle-11g-example-xml-mapping/)
*   [休眠-类型注释配置为 de](/web/20210109151106/https://mkyong.com/hibernate/hibernate-the-type-annotationconfiguration-is-deprecated/)