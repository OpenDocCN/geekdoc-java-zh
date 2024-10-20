# ant——如何从路径 id 打印类路径

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/ant/ant-how-to-print-classpath-from-path-id/>

在 Ant 中，可以使用`pathconvert`任务从 path 中打印出类路径:

build.xml

```java
 <path id="project.web.classpath">
	<pathelement location="test/lib/junit-4.11.jar"/>
	<pathelement location="test/lib/hamcrest-core-1.3.jar"/>

	<fileset dir="${lib.web.dir}">
		<include name="**/*.jar" />
	</fileset>	
  </path>

  <target name="print-classpath">

	<pathconvert property="classpathInName" refid="project.web.classpath" />
	<echo>Classpath is ${classpathInName}</echo>

  </target> 
```

测试:

```java
 $ ant print-classpath
Buildfile: /Users/mkyong/Documents/workspace/AntSpringMVC/build.xml

print-classpath:

     [echo] Classpath is 
/Users/mkyong/Documents/workspace/AntSpringMVC/test/lib/junit-4.11.jar:
/Users/mkyong/Documents/workspace/AntSpringMVC/testlib/hamcrest-core-1.3.jar:
......

BUILD SUCCESSFUL
Total time: 0 seconds 
```

## 参考

1.  [蚂蚁路径转换任务](http://web.archive.org/web/20190115040926/https://ant.apache.org/manual/Tasks/pathconvert.html)

[ant](http://web.archive.org/web/20190115040926/http://www.mkyong.com/tag/ant/)![](img/a50c366e57aeb2cc18187f9753cb9c49.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190115040926/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="13547">







