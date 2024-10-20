# 如何在 CentOS 上安装 Oracle JDK 8

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-install-oracle-jdk-8-on-centos/>

![java8-centos-example](img/3204163e8bb54669261d7ab03f4a7ac4.png)

在本教程中，我们将向您展示如何在 CentOS 上安装 Oracle JDK 8。

环境:

1.  厘斯 6.8
2.  甲骨文 JDK 8u102

**Note**
This guide should work on Fedora and RedHat.

## 1.获取甲骨文 JDK 8

1.1 访问[甲骨文 JDK 下载页面](http://web.archive.org/web/20210506220324/http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)，寻找`RPM`版本。

1.2 复制`jdk-8u102-linux-x64.rpm`和`wget`的下载链接。

```java
 $ pwd
/home/mkyong

$ wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u102-b14/jdk-8u102-linux-x64.rpm 
```

## 2.安装 Oracle JDK 8

2.1 用`yum localinstall`安装。

```java
 $ sudo yum localinstall jdk-8u102-linux-x64.rpm

//...
//...
//...
Installed:
  jdk1.8.0_102.x86_64 2000:1.8.0_102-fcs                                                                                                                                          

Complete! 
```

2.2 现在 JDK 应该安装在`/usr/java/jdk1.8.0_102`

```java
 $ cd /usr/java
$ ls -lsah
total 12K
4.0K drwxr-xr-x   3 root root 4.0K Jul 21 09:58 ./
4.0K drwxr-xr-x. 15 root root 4.0K Jun 22 22:00 ../
   0 lrwxrwxrwx   1 root root   16 Jul 21 09:58 default -> /usr/java/latest/
4.0K drwxr-xr-x   9 root root 4.0K Jul 21 09:58 jdk1.8.0_102/
   0 lrwxrwxrwx   1 root root   22 Jul 21 09:58 latest -> /usr/java/jdk1.8.0_102/ 
```

2.3 验证

```java
 $ java -version
java version "1.8.0_102"
Java(TM) SE Runtime Environment (build 1.8.0_102-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.102-b14, mixed mode) 
```

2.4 删除 RPM 文件

```java
 $ rm ~/jdk-8u102-linux-x64.rpm 
```

完成了。Oracle JDK 8 成功安装在 CentOS 上。

## 3.JAVA_HOME 环境变量

这是设置环境变量`JAVA_HOME`的好方法。

3.1 编辑`.bash_profile`，在文件末尾追加`export JAVA_HOME`，例如:

.bash_profile

```java
 # Get the aliases and functions
if [ -f ~/.bashrc ]; then
	. ~/.bashrc
fi

# User specific environment and startup programs

export JAVA_HOME=/usr/java/jdk1.8.0_102/
export JRE_HOME=/usr/java/jdk1.8.0_102/jre

PATH=$PATH:$HOME/bin:$JAVA_HOME/bin

export PATH 
```

3.2 测试`$JAVA_HOME`和`$PATH`

```java
 $ source .bash_profile 

$ echo $JRE_HOME
/usr/java/jdk1.8.0_102/jre

$ echo $JAVA_HOME
/usr/java/jdk1.8.0_102/

$ echo $PATH
/...:/usr/local/bin:/usr/X11R6/bin:/home/mkyong/bin:/usr/java/jdk1.8.0_102//bin 
```

## 4.安装了多个 JDK

如果 CentOS 安装了多个 JDK，您可以使用`alternatives`命令设置默认的`java`

```java
 $ sudo alternatives --config java
[sudo] password for mkyong: 

There are 2 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
   1           /usr/lib/jvm/jre-1.6.0-openjdk.x86_64/bin/java
*+ 2           /usr/java/jdk1.8.0_102/jre/bin/java

Enter to keep the current selection[+], or type selection number: 
```

## 参考

1.  [如何在 Debian 上安装 Oracle JDK 8](http://web.archive.org/web/20210506220324/http://www.mkyong.com/java/how-to-install-oracle-jdk-8-on-debian/)
2.  [如何在 CentOS 和 Fedora 上安装 Java](http://web.archive.org/web/20210506220324/https://www.digitalocean.com/community/tutorials/how-to-install-java-on-centos-and-fedora)
3.  [如何在 Linux 或 CentOS 中设置 JAVA 环境变量](http://web.archive.org/web/20210506220324/http://sharadchhetri.com/2013/06/03/how-to-set-java-environment-variables-in-linux-or-centos/)
4.  [如何使用‘alternatives’命令在 CentOS/RHEL 和 Fedora 上安装 Java 8](http://web.archive.org/web/20210506220324/http://tecadmin.net/install-java-8-on-centos-rhel-and-fedora/)
5.  [Fedora、Redhat 和 CentOS 的区别](http://web.archive.org/web/20210506220324/https://danielmiessler.com/study/fedora_redhat_centos/)

Tags : [CentOS](http://web.archive.org/web/20210506220324/https://mkyong.com/tag/centos/) [fedora](http://web.archive.org/web/20210506220324/https://mkyong.com/tag/fedora/) [install-java](http://web.archive.org/web/20210506220324/https://mkyong.com/tag/install-java/) [java8](http://web.archive.org/web/20210506220324/https://mkyong.com/tag/java8/) [RedHat](http://web.archive.org/web/20210506220324/https://mkyong.com/tag/redhat/)<input type="hidden" id="mkyong-current-postId" value="14022">