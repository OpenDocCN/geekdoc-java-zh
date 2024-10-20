# Maven 本地存储库在哪里？

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/maven/where-is-maven-local-repository/>

默认情况下，Maven 本地存储库默认为`${user.home}/.m2/repository`文件夹:

1.  UNIX/Mac OS X-`~/.m2/repository`
2.  windows—`C:\Users\{your-username}\.m2\repository`

当我们编译一个 Maven 项目时，Maven 会将项目的所有依赖项和插件 jar 下载到 Maven 本地存储库中，为下一次编译节省时间。

## 1.查找 Maven 本地存储库

1.1 如果默认`.m2`找不到，可能有人改变了默认路径。发出以下命令，找出 Maven 本地存储库在哪里:

```java
 mvn help:evaluate -Dexpression=settings.localRepository 
```

1.2 示例:

Terminal

```java
 D:\> mvn help:evaluate -Dexpression=settings.localRepository

[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-help-plugin:3.1.0:evaluate (default-cli) @ standalone-pom ---
[INFO] No artifact parameter specified, using 'org.apache.maven:standalone-pom:pom:1' as project.
[INFO]

C:\opt\maven-repository

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.598 s
[INFO] Finished at: 2018-10-24T16:44:18+08:00
[INFO] ------------------------------------------------------------------------ 
```

在上面的输出中，Maven 本地存储库被重新定位到`C:\opt\maven-repository`

## 2.更新 Maven 本地存储库

2.1 找到这个文件`{MAVEN_HOME}\conf\settings.xml`并更新`localRepository`。

{MAVEN_HOME}\conf\settings.xml

```java
 <settings>
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ~/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->

<localRepository>D:/maven_repo</localRepository> 
```

**Note**
Issue `mvn -version` to find out where is Maven installed.

2.2 保存文件，完成，Maven 本地库现在改为`D:/maven_repo`。

![](img/88b2f13f8c3ac92fc21c516efe3b930d.png)

## 参考

1.  [Maven 中央储存库](http://web.archive.org/web/20210815110733/https://maven.apache.org/repository/index.html)
2.  [存储库简介](http://web.archive.org/web/20210815110733/https://maven.apache.org/guides/introduction/introduction-to-repositories.html)

Tags : [maven](http://web.archive.org/web/20210815110733/https://mkyong.com/tag/maven/) [maven repository](http://web.archive.org/web/20210815110733/https://mkyong.com/tag/maven-repository/) [maven-faq](http://web.archive.org/web/20210815110733/https://mkyong.com/tag/maven-faq/) [maven-repo](http://web.archive.org/web/20210815110733/https://mkyong.com/tag/maven-repo/)<input type="hidden" id="mkyong-current-postId" value="805">