# 如何用 Hibernate 工具生成 Hibernate 映射文件和注释

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/how-to-generate-code-with-hibernate-tools/>

在本文中，我们将向您展示如何使用 **Hibernate / JBoss Tools** 从数据库自动生成 Hibernate 映射文件(hbm)和注释代码。

本文中的工具

1.  Eclipse v3.6 (Helios)
2.  JBoss / Hibernate 工具 3.2 版
3.  Oracle 11g
4.  JDK 1.6

**Note**
Before proceed, please [Install Hibernate / JBoss Tools in Eclipse IDE](http://web.archive.org/web/20220709170541/http://www.mkyong.com/hibernate/how-to-install-hibernate-tools-in-eclipse-ide/).

## 1.Hibernate 透视图

打开你的**休眠视角**。在 Eclipse IDE 中，选择“**Windows**”>>**打开透视图**>>**其他…** ，选择“**休眠**”。

## 2.新的休眠配置

在 Hibernate 透视图中，右键选择“**添加配置…** ”

在“编辑配置”对话框中，

1.  在**项目**框中，点击【浏览..】按钮选择您的项目。
2.  在“**数据库连接**框中，点击“新建..”按钮来创建数据库设置。
3.  在**配置文件**框中，点击【设置】按钮创建一个新的或者使用已有的【休眠配置文件】，`hibernate.cfg.xml`。

![Eclipse Hibernate Tools](img/4bf514ea776eac7d93fd071c47742814.png "Eclipse-Hibernate-1")

在“ **Hibernate 透视图**”中查看您的表列表。

![Eclipse Hibernate Tools](img/204e6800cebce5d4834aa2d2acd61a20.png "Eclipse-Hibernate-2")

“`hibernate.cfg.xml`”示例，连接到 Oracle 11g 数据库。

```java
 <?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
<session-factory>
  <property name="hibernate.connection.driver_class">oracle.jdbc.driver.OracleDriver</property>
  <property name="hibernate.connection.url">jdbc:oracle:thin:@127.0.0.1:1521:MKYONG</property>
  <property name="hibernate.connection.username">mkyong</property>
  <property name="hibernate.connection.password">password</property>
  <property name="hibernate.dialect">org.hibernate.dialect.Oracle10gDialect</property>
  <property name="hibernate.default_schema">MKYONG</property>
 </session-factory>
</hibernate-configuration> 
```

## 3.Hibernate 代码生成

现在，您已经准备好生成 Hibernate 映射文件和注释代码了。

–在“Hibernate 透视图”中，点击“ **Hibernate 代码生成**”图标(见下图)，选择“Hibernate 代码生成配置”

![Hibernate Code Generation](img/3d7cde713fe62e2856ba448ae035a70b.png "Hibernate-Code-Generation-1")

–创建一个新配置，选择您的“**控制台配置**”(在步骤 2 中配置)，放置您的“**输出目录**，选中选项“**从 JDBC 连接反向工程**”。

![Hibernate Code Generation](img/1d79e4f0a5f627966737cba117ee766e.png "Hibernate-Code-Generation-2")

–在“ **Exporter** 选项卡中，选择要生成的内容、模型、映射文件(hbm)、DAO、注释代码等。

![Hibernate Code Generation](img/cb99286f0e5063053965c67fa4cbb61b.png "Hibernate-Code-Generation-3")

看到结果

![Hibernate Code Generation](img/ed8d352bb70e11a3b07b653f1a2be235.png "Hibernate-Code-Generation-4")**Note**
The generated Hibernate mapping file and annotations code are very clean, standard and easy to modify. Try explore more features.<input type="hidden" id="mkyong-current-postId" value="2384">