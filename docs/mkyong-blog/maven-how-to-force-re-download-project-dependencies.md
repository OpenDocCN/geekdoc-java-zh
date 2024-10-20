# maven–如何强制重新下载项目依赖关系？

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/maven/maven-how-to-force-re-download-project-dependencies/>

在 Maven 中，你可以使用 [Apache Maven 依赖插件](http://web.archive.org/web/20210814213520/https://maven.apache.org/plugins/maven-dependency-plugin/index.html)，目标`dependency:purge-local-repository`从本地存储库中移除项目依赖，并再次重新下载。

Terminal

```java
 $ mvn dependency:purge-local-repository

[INFO] Scanning for projects...
[INFO]
[INFO] --------------< com.mkyong.examples:maven-code-coverage >---------------
[INFO] Building maven-code-coverage 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:purge-local-repository (default-cli) @ maven-code-coverage ---
Downloading from central: https://repo.maven.apache.org/maven2/org/junit/jupiter/junit-jupiter-engine/5.3.1/junit-jupiter-engine-5.3.1.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/junit/jupiter/junit-jupiter-engine/5.3.1/junit-jupiter-engine-5.3.1.pom (2.4 kB at 2.1 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apiguardian/apiguardian-api/1.0.0/apiguardian-api-1.0.0.pom

//...

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  8.086 s
[INFO] Finished at: 2018-11-20T15:22:28+08:00
[INFO] ------------------------------------------------------------------------ 
```

## 参考

1.  [Maven 依赖:清除-本地-存储库](http://web.archive.org/web/20210814213520/https://maven.apache.org/plugins/maven-dependency-plugin/purge-local-repository-mojo.html)

Tags : [maven](http://web.archive.org/web/20210814213520/https://mkyong.com/tag/maven/) [maven-faq](http://web.archive.org/web/20210814213520/https://mkyong.com/tag/maven-faq/) [project dependency](http://web.archive.org/web/20210814213520/https://mkyong.com/tag/project-dependency/)<input type="hidden" id="mkyong-current-postId" value="14819">