# 构建 Java 项目的 Ant 模板文件

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/ant/ant-template-file-to-build-a-java-project/>

这里有一个 Apache Ant 模板文件，最适合用来从头开始一个项目。

*文件:build.xml*

```java
 <project name="ProjectName" default="dist" basedir=".">
    <description>
        Project Description
    </description>

  <!-- set global properties for this build -->
  <property name="projectName" value="ProjectName"/>
  <property name="src" location="src"/>
  <property name="build" location="build"/>
  <property name="dist"  location="dist"/>
  <property name="webcontent"  location="WebContent"/>

  <target name="init">
    	<!-- Create the time stamp -->
   	 <tstamp/>
    	<!-- Create the build directory structure used by compile -->
    	<mkdir dir="${build}"/>
  </target>

  <target name="compile" depends="init"
  	description="compile the source " >
    	<!-- Compile the java code from ${src} into ${build} -->
   	<javac srcdir="${src}" destdir="${build}"/>
  </target>

  <target name="dist" depends="compile"
  	description="generate the distribution" >

  	<!-- Create the distribution directory -->
    	<mkdir dir="${dist}/lib"/>

    	<!-- Put everything in ${build} into the {$projectName}-${DSTAMP}.jar file -->
    	<jar jarfile="${dist}/lib/${projectName}-${DSTAMP}.jar" basedir="${build}"/>
  </target>

  <target name="war" depends="compile"
  	description="generate the distribution war" >

	<!-- Create the war distribution directory -->
  	<mkdir dir="${dist}/war"/>

  	<!-- Follow standard WAR structure -->
  	<copydir dest="${dist}/war/build/WEB-INF/" src="${webcontent}/WEB-INF/" />
  	<copydir dest="${dist}/war/build/WEB-INF/classes/" src="${build}" />

	<jar jarfile="${dist}/war/${projectName}-${DSTAMP}.war" basedir="${dist}/war/build/"/>
  </target>

  <target name="clean"
    	description="clean up" >

    	<!-- Delete the ${build} and ${dist} directory trees -->
    	<delete dir="${build}"/>
    	<delete dir="${dist}"/>
  </target>
</project> 
```

<input type="hidden" id="mkyong-current-postId" value="7819">