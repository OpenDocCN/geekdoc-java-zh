# Gradle 和 JUnit 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/gradle-and-junit-example/>

在 Gradle 中，您可以像这样声明 JUnit 依赖关系:

build.gradle

```java
 apply plugin: 'java'

	dependencies {
		testCompile 'junit:junit:4.12'
	} 
```

默认情况下，JUnit 附带了一个`hamcrest-core`的捆绑副本

```java
 $ gradle dependencies --configuration testCompile

testCompile - Compile classpath for source set 'test'.
\--- junit:junit:4.12
     \--- org.hamcrest:hamcrest-core:1.3 
```

## 1.Gradle + JUnit + Hamcrest

通常，我们需要有用的`hamcrest-library`库，所以，最好排除`hamcrest-core`的 JUnit 捆绑副本，包括原始的`hamcrest-core`库。再次回顾更新后的`pom.xml`。

build.gradle

```java
 apply plugin: 'java'

	dependencies {
		testCompile('junit:junit:4.12'){
			exclude group: 'org.hamcrest'
		}
		testCompile 'org.hamcrest:hamcrest-library:1.3'
	} 
```

再次检查依赖性。

```java
 $ gradle dependencies --configuration testCompile

testCompile - Compile classpath for source set 'test'.
+--- junit:junit:4.12
\--- org.hamcrest:hamcrest-library:1.3
     \--- org.hamcrest:hamcrest-core:1.3 
```

 ## 参考

1.  [显示项目依赖关系](http://web.archive.org/web/20190223082648/http://www.mkyong.com/gradle/gradle-display-project-dependency/)
2.  [JUnit–与 Gradle 一起使用](http://web.archive.org/web/20190223082648/https://github.com/junit-team/junit4/wiki/Use-with-Gradle)

[gradle](http://web.archive.org/web/20190223082648/http://www.mkyong.com/tag/gradle/) [hamcrest](http://web.archive.org/web/20190223082648/http://www.mkyong.com/tag/hamcrest/) [junit](http://web.archive.org/web/20190223082648/http://www.mkyong.com/tag/junit/)![](img/58182f7b5263ff3130ef7d7aa3f4cada.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190223082648/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="13984">







