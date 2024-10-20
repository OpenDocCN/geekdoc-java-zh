# 如何在 Ubuntu 上安装 Maven

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/maven/how-to-install-maven-in-ubuntu/>

在本教程中，我们将向您展示如何在 Ubuntu 上安装 Apache Maven。

测试对象

1.  Maven 3.5.2
2.  Ubuntu 18.04

**Note**
Before the Maven installation, make sure [JDK is installed and JAVA_HOME is configured](http://web.archive.org/web/20220719083042/http://www.mkyong.com/java/how-to-install-java-jdk-on-ubuntu-linux/).

## 1.搜索专家

查看本地存储库中的 Maven 包版本。

```java
 $ sudo apt policy maven

maven:
  Installed: (none)
  Candidate: 3.5.2-2
  Version table:
     3.5.2-2 500
        500 http://my.archive.ubuntu.com/ubuntu bionic/universe amd64 Packages
        500 http://my.archive.ubuntu.com/ubuntu bionic/universe i386 Packages 
```

是 Maven 3.5.2。

## 2.安装 Maven

通过`apt`命令安装 Maven。

```java
 $ sudo apt install maven

Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libaopalliance-java libapache-pom-java libatinject-

//...

Setting up libsisu-inject-java (0.3.2-2) ...
Setting up libsisu-plexus-java (0.3.3-3) ...
Setting up libmaven3-core-java (3.5.2-2) ...
Setting up maven (3.5.2-2) ...
update-alternatives: using /usr/share/maven/bin/mvn to provide /usr/bin/mvn (mvn) in auto mode 
```

## 3.确认

Apache Maven 3.5.2 安装成功。

```java
 $ mvn -version

Apache Maven 3.5.2
Maven home: /usr/share/maven
Java version: 11.0.1, vendor: Oracle Corporation
Java home: /usr/lib/jvm/java-11-oracle
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.15.0-38-generic", arch: "amd64", family: "unix" 
```

## 4.玛文在哪里？

`apt`命令在以下位置安装了 Maven:

```java
 $ ls -lsa /usr/share/maven
total 32
 4 drwxr-xr-x   6 root root  4096 Nov   9 17:34 .
12 drwxr-xr-x 227 root root 12288 Nov   9 17:34 ..
 4 drwxr-xr-x   2 root root  4096 Nov   9 17:34 bin
 4 drwxr-xr-x   2 root root  4096 Nov   9 17:34 boot
 0 lrwxrwxrwx   1 root root    10 Feb  24  2018 conf -> /etc/maven
 4 drwxr-xr-x   2 root root  4096 Nov   9 17:34 lib
 4 drwxr-xr-x   2 root root  4096 Nov   9 17:34 man

$ ls -lsa /etc/maven
total 40
 4 drwxr-xr-x   3 root root  4096 Nov   9 17:34 .
12 drwxr-xr-x 127 root root 12288 Nov   9 17:34 ..
 4 drwxr-xr-x   2 root root  4096 Nov   9 17:34 logging
 4 -rw-r--r--   1 root root   220 Okt  18  2017 m2.conf
12 -rw-r--r--   1 root root 10211 Okt  18  2017 settings.xml
 4 -rw-r--r--   1 root root  3645 Okt  18  2017 toolchains.xml 
```

## 参考

1.  [如何在 Ubuntu 上安装 Java JDK](http://web.archive.org/web/20220719083042/http://www.mkyong.com/java/how-to-install-java-jdk-on-ubuntu-linux/)

<input type="hidden" id="mkyong-current-postId" value="2105">