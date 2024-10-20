# JUnit——如何测试地图

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/junit-how-to-test-a-map/>

忘掉 JUnit `assertEquals()`，为了测试一个`Map`，使用了来自`hamcrest-library.jar`的更有表现力的`IsMapContaining`类

pom.xml

```java
 <dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.hamcrest</groupId>
					<artifactId>hamcrest-core</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<!-- This will get hamcrest-core automatically -->
		<dependency>
			<groupId>org.hamcrest</groupId>
			<artifactId>hamcrest-library</artifactId>
			<version>1.3</version>
			<scope>test</scope>
		</dependency>
	</dependencies> 
```

## 1.包含示例的 ismap

以下所有`assertThat`检查都将通过。

MapTest.java

```java
 package com.mkyong;

import org.hamcrest.collection.IsMapContaining;
import org.junit.Test;

import java.util.HashMap;
import java.util.Map;

import static org.hamcrest.CoreMatchers.is;
import static org.hamcrest.CoreMatchers.not;
import static org.hamcrest.MatcherAssert.assertThat;

public class MapTest {

    @Test
    public void testAssertMap() {

        Map<String, String> map = new HashMap<>();
        map.put("j", "java");
        map.put("c", "c++");
        map.put("p", "python");
        map.put("n", "node");

        Map<String, String> expected = new HashMap<>();
        expected.put("n", "node");
        expected.put("c", "c++");
        expected.put("j", "java");
        expected.put("p", "python");

        //All passed / true

        //1\. Test equal, ignore order
        assertThat(map, is(expected));

        //2\. Test size
        assertThat(map.size(), is(4));

        //3\. Test map entry, best!
        assertThat(map, IsMapContaining.hasEntry("n", "node"));

        assertThat(map, not(IsMapContaining.hasEntry("r", "ruby")));

        //4\. Test map key
        assertThat(map, IsMapContaining.hasKey("j"));

        //5\. Test map value
        assertThat(map, IsMapContaining.hasValue("node"));

    }

} 
```

**Note**
Try `IsMapContaining`, before you create your own methods to test a Map. ## 参考

1.  [Hamcrest 官方网站](http://web.archive.org/web/20190214234119/http://hamcrest.org/)
2.  [正在包含 JavaDoc](http://web.archive.org/web/20190214234119/http://hamcrest.org/JavaHamcrest/javadoc/1.3/org/hamcrest/collection/IsMapContaining.html)
3.  [JUnit–如何测试列表](http://web.archive.org/web/20190214234119/http://www.mkyong.com/unittest/junit-how-to-test-a-list/)

[hamcrest](http://web.archive.org/web/20190214234119/http://www.mkyong.com/tag/hamcrest/) [junit](http://web.archive.org/web/20190214234119/http://www.mkyong.com/tag/junit/) [map](http://web.archive.org/web/20190214234119/http://www.mkyong.com/tag/map/)![](img/63494b4c06ff339aeaddf4441a03ce92.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214234119/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="13987">







