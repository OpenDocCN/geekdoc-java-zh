# 从资源文件夹中读取文件

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring/spring-read-file-from-resources-folder/>

在 Spring 中，我们可以使用`ClassPathResource`或`ResourceLoader`轻松地从类路径中获取文件。

*用弹簧 5.1.4 测试 P.S .释放*

## 1.资源中心/主要/资源/

例如，`src/main/resources/`文件夹中的图像文件

![resources](img/4c9f41af2a7b1e7f0cbf95ef7eb02329.png)

## 2.ClassPathResource

```java
 import org.springframework.core.io.Resource;
import org.springframework.core.io.ClassPathResource;

import java.io.File;
import java.io.InputStream;

	Resource resource = new ClassPathResource("android.png");

	InputStream input = resource.getInputStream();

	File file = resource.getFile(); 
```

## 3.资源加载器

```java
 import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;

import java.io.File;
import java.io.InputStream;

	@Autowired
    ResourceLoader resourceLoader;

	Resource resource = resourceLoader.getResource("classpath:android.png");

	InputStream input = resource.getInputStream();

	File file = resource.getFile(); 
```

## 4.资源工具

请不要使用这个`ResourceUtils`即使它有效，这个类主要是在框架内部使用。阅读 [ResourceUtils JavaDocs](http://web.archive.org/web/20221127035738/https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/ResourceUtils.html)

```java
 import org.springframework.util.ResourceUtils;

	File file = ResourceUtils.getFile("classpath:android.png"); 
```

# 参考

*   [class path resource JavaDocs](http://web.archive.org/web/20221127035738/https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/core/io/ClassPathResource.html)
*   [资源加载器 JavaDocs](http://web.archive.org/web/20221127035738/https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/io/ResourceLoader.html)
*   [ResourceUtils JavaDocs](http://web.archive.org/web/20221127035738/https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/ResourceUtils.html)
*   [Java–从资源文件夹中读取文件](http://web.archive.org/web/20221127035738/https://www.mkyong.com/java/java-read-a-file-from-resources-folder/)

<input type="hidden" id="mkyong-current-postId" value="15041">