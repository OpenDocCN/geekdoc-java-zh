# 休眠–将图像保存到数据库中

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-save-image-into-database/>

要将图像保存到数据库中，您需要在 MySQL 中将表列定义为 blob 数据类型，或者在其他数据库中定义为等效的二进制类型。在 Hibernate 端，你可以声明一个字节数组变量来存储图像数据。

Download this example – [Hibernate-Image-Example.zip](http://web.archive.org/web/20210426220336/http://www.mkyong.com/wp-content/uploads/2010/03/Hibernate-Image-Example.zip)

下面是一个 Maven 项目，它使用 Hibernate 将图像保存到 MySQL ' **avatar** 表中。

## 1.表格创建

MySQL 中的头像表创建脚本。

```java
 CREATE TABLE  `mkyong`.`avatar` (
  `AVATAR_ID` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `IMAGE` blob NOT NULL,
  PRIMARY KEY (`AVATAR_ID`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8; 
```

## 2.Maven 依赖性

添加 Hibernate 和 MySQL 依赖。

**pom.xml**

```java
 <project  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
  http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mkyong.common</groupId>
  <artifactId>HibernateExample</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>HibernateExample</name>
  <url>http://maven.apache.org</url>
  <dependencies>

        <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>3.8.1</version>
               <scope>test</scope>
        </dependency>

        <!-- MySQL database driver -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>5.1.9</version>
	</dependency>

	<!-- Hibernate framework -->
	<dependency>
		<groupId>hibernate</groupId>
		<artifactId>hibernate3</artifactId>
		<version>3.2.3.GA</version>
	</dependency>

	<!-- Hibernate library dependecy start -->
	<dependency>
		<groupId>dom4j</groupId>
		<artifactId>dom4j</artifactId>
		<version>1.6.1</version>
	</dependency>

	<dependency>
		<groupId>commons-logging</groupId>
		<artifactId>commons-logging</artifactId>
		<version>1.1.1</version>
	</dependency>

	<dependency>
		<groupId>commons-collections</groupId>
		<artifactId>commons-collections</artifactId>
		<version>3.2.1</version>
	</dependency>

	<dependency>
		<groupId>cglib</groupId>
		<artifactId>cglib</artifactId>
		<version>2.2</version>
	</dependency>
	<!-- Hibernate library dependecy end -->

  </dependencies>
</project> 
```

## 3.化身模型

创建一个模型类来存储头像数据。图像数据类型是字节数组。

**Avatar.java**

```java
 package com.mkyong.common;

public class Avatar implements java.io.Serializable {

	private Integer avatarId;
	private byte[] image;

	public Avatar() {
	}

	public Avatar(byte[] image) {
		this.image = image;
	}

	public Integer getAvatarId() {
		return this.avatarId;
	}

	public void setAvatarId(Integer avatarId) {
		this.avatarId = avatarId;
	}

	public byte[] getImage() {
		return this.image;
	}

	public void setImage(byte[] image) {
		this.image = image;
	}

} 
```

## 4.映射文件

为 avatar 创建一个 Hibernate 映射文件。图像的数据类型是二进制。

**Avatar.hbm.xml**

```java
 <?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
    <class name="com.mkyong.common.Avatar" table="avatar" catalog="mkyong">
        <id name="avatarId" type="java.lang.Integer">
            <column name="AVATAR_ID" />
            <generator class="identity" />
        </id>
        <property name="image" type="binary">
            <column name="IMAGE" not-null="true" />
        </property>
    </class>
</hibernate-mapping> 
```

## 5.休眠配置文件

定义数据库连接的 Hibernate 配置文件和 Hibernate 映射文件。

**hibernate.cfg.xml**

```java
 <?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <property name="hibernate.bytecode.use_reflection_optimizer">false</property>
        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="hibernate.connection.password">password</property>
        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/mkyong</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        <property name="show_sql">true</property>
        <mapping resource="com/mkyong/common/Avatar.hbm.xml"></mapping>
    </session-factory>
</hibernate-configuration> 
```

## 6.休眠实用程序

用于获取数据库连接的 Hibernate 实用程序类。

**HibernateUtil.java**

```java
 package com.mkyong.persistence;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateUtil {

    private static final SessionFactory sessionFactory = buildSessionFactory();

    private static SessionFactory buildSessionFactory() {
        try {
            // Create the SessionFactory from hibernate.cfg.xml
            return new Configuration().configure().buildSessionFactory();
        }
        catch (Throwable ex) {
            // Make sure you log the exception, as it might be swallowed
            System.err.println("Initial SessionFactory creation failed." + ex);
            throw new ExceptionInInitializerError(ex);
        }
    }

    public static SessionFactory getSessionFactory() {
        return sessionFactory;
    }

    public static void shutdown() {
    	// Close caches and connection pools
    	getSessionFactory().close();
    }

} 
```

## 7.运行它

读取一个文件“**C:\ \ mavan-hibernate-image-MySQL . gif**”并保存到数据库中，之后从数据库中获取并保存到另一个镜像文件“ **C:\\test.gif** ”。

```java
 package com.mkyong.common;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;

import org.hibernate.Session;
import com.mkyong.persistence.HibernateUtil;

public class App 
{
    public static void main( String[] args )
    {
        System.out.println("Hibernate save image into database");
        Session session = HibernateUtil.getSessionFactory().openSession();

        session.beginTransaction();

        //save image into database
    	File file = new File("C:\\mavan-hibernate-image-mysql.gif");
        byte[] bFile = new byte[(int) file.length()];

        try {
	     FileInputStream fileInputStream = new FileInputStream(file);
	     //convert file into array of bytes
	     fileInputStream.read(bFile);
	     fileInputStream.close();
        } catch (Exception e) {
	     e.printStackTrace();
        }

        Avatar avatar = new Avatar();
        avatar.setImage(bFile);

        session.save(avatar);

        //Get image from database
        Avatar avatar2 = (Avatar)session.get(Avatar.class, avatar.getAvatarId());
        byte[] bAvatar = avatar2.getImage();

        try{
            FileOutputStream fos = new FileOutputStream("C:\\test.gif"); 
            fos.write(bAvatar);
            fos.close();
        }catch(Exception e){
            e.printStackTrace();
        }

        session.getTransaction().commit();
    }
} 
```

完成了。

Tags : [hibernate](http://web.archive.org/web/20210426220336/https://mkyong.com/tag/hibernate/) [image](http://web.archive.org/web/20210426220336/https://mkyong.com/tag/image/)<input type="hidden" id="mkyong-current-postId" value="4158">