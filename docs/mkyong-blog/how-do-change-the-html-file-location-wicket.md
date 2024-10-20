# 如何在 Wicket 中改变 html 文件的位置

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/how-do-change-the-html-file-location-wicket/>

Wicket 要求 html 和 java 文件位于同一个包目录中。这里我们向你展示如何把 java 和 html 文件放到不同的目录中。

举个例子，

1.  Index.java 位于项目**/src/main/Java/com/mkyong**
2.  Index.Html 位于项目 **/src/main/webapp/pages** ，根上下文“/”位于“/src/main/webapp”。

*显示动作中的便门*

> Wicket 定位资源的默认方式使您能够在开发过程中在 Java 文件和标记文件之间快速切换，因为它们紧挨着。此外，使用这种算法，您的包组件可以立即重用，而无需用户配置模板的加载位置；如果可以在类路径中找到组件的类，那么也可以找到它们的资源。这是一个强大的默认设置，在您实现自定义的东西之前，您可能要三思而行

## 检票口 1.3

如果您仍然坚持定制资源路径，这里我提供了 2 种在 Wicket 1.3 中实现的方法。

 ## 1.使用 web 上下文定位资源

创建一个扩展 **ResourceStreamLocator** 类的类，并覆盖以下函数

1.  定位(类别分类，字符串路径)

```java
 package com.mkyong;

import java.net.MalformedURLException;
import java.net.URL;

import org.apache.wicket.WicketRuntimeException;
import org.apache.wicket.protocol.http.WebApplication;
import org.apache.wicket.util.resource.IResourceStream;
import org.apache.wicket.util.resource.UrlResourceStream;
import org.apache.wicket.util.resource.locator.ResourceStreamLocator;
import org.apache.wicket.util.string.Strings;

public class MyOwnStreamLocator extends ResourceStreamLocator
{
	@Override
	public IResourceStream locate(Class<?> clazz, String path)
	{

		String location;

		String extension = path.substring(path.lastIndexOf('.') + 1);
		String simpleFileName = Strings.lastPathComponent(clazz.getName(), '.');
		location = "/pages/" + simpleFileName + "." + extension;

		URL url;
		try
		{
			// try to load the resource from the web context
			url = WebApplication.get().getServletContext().getResource(location);

			if (url != null)
			{
				return new UrlResourceStream(url);
			}
		}
		catch (MalformedURLException e)
		{
			throw new WicketRuntimeException(e);
		}

		// resource not found; fall back on class loading
		return super.locate(clazz, path);
	}

} 
```

Wicket 应用程序类

```java
 package com.mkyong;

import org.apache.wicket.protocol.http.WebApplication;
import org.apache.wicket.util.file.WebApplicationPath;

public class MyApplication extends WebApplication
{    
	public Class<Index> getHomePage()
	{
		return Index.class;
	}

	@Override
	protected void init() {

        getResourceSettings().setResourceStreamLocator(new MyOwnStreamLocator());

	}

} 
```

 ## 2.使用 ResourceFinder 定位资源

创建一个扩展 **ResourceStreamLocator** 类的类，并覆盖以下两个函数

1.  定位(类别、字符串路径、字符串样式、区域设置、字符串扩展名)
2.  locateByResourceFinder(类 clazz，字符串路径)

```java
 package com.mkyong;

import java.util.Locale;

import org.apache.wicket.util.file.IResourceFinder;
import org.apache.wicket.util.resource.IResourceStream;
import org.apache.wicket.util.resource.locator.ResourceStreamLocator;
import org.apache.wicket.util.string.Strings;

public class MyOwnFinderStreamLocator extends ResourceStreamLocator
{

	private IResourceFinder resourceFinder;

	public void addFinder(IResourceFinder resourceFinder) {
		if (resourceFinder != null) {
			this.resourceFinder = resourceFinder;
		}
	}

	@Override
	public IResourceStream locate(Class<?> clazz, String path, String style,
		Locale locale, String extension) {

		String simpleFileName = 
                Strings.lastPathComponent(clazz.getName(), '.') + "." + extension;

		IResourceStream stream = locate(clazz, simpleFileName);

		if(stream!=null)
			return stream;
		else
			return super.locate(clazz, path,style,locale,extension);

	}

	@Override
	protected IResourceStream locateByResourceFinder(Class<?> clazz, String path) {
		IResourceStream resourceStream = null;

		resourceStream = resourceFinder.find(clazz, path);

		if (resourceStream == null) {
			// try by using class loader
			resourceStream = locateByClassLoader(clazz, path);
		}

		return resourceStream;
	}
} 
```

Wicket 应用程序类

```java
 package com.mkyong;

import org.apache.wicket.protocol.http.WebApplication;
import org.apache.wicket.util.file.WebApplicationPath;

public class MyApplication extends WebApplication
{    
	public Class<Index> getHomePage()
	{
		return Index.class;
	}

	@Override
	protected void init() {

	  //resource finder
	  MyOwnFinderStreamLocator resourceStreamLocator = new MyOwnFinderStreamLocator();

	  WebApplicationPath webContainerPathFinder = new WebApplicationPath(getServletContext());
	  webContainerPathFinder.add("/pages/");
	  resourceStreamLocator.addFinder(webContainerPathFinder);

	  getResourceSettings().setResourceStreamLocator(resourceStreamLocator);
	}
} 
```

完成了。

Download It – [Wicket-1.3-Resource-Loading.zip](http://web.archive.org/web/20190306164340/http://www.mkyong.com/wp-content/uploads/2009/02/resource-loading.zip) (7KB)

## 检票口 1.4

对于 Wicket 1.4，就更简单了。请参见以下目录结构:

1.  Hello.java 在/src/main/Java/com/mkyong/hello
2.  Hello.html 在/src/main/web app/pages/com/mkyong/hello

![wicket change html location ](img/203f8d3c581edb1fe76ff3a75266f142.png "wicket-example-change-html-location")

您可以自定义 html 在 **init()** 方法中的加载位置，如下所示:

```java
 package com.mkyong;

import org.apache.wicket.Page;
import org.apache.wicket.protocol.http.WebApplication;
import org.apache.wicket.settings.IResourceSettings;
import com.mkyong.hello.Hello;

public class MyApplication extends WebApplication {

	@Override
	public Class<? extends Page> getHomePage() {
		return Hello.class; //return default page
	}

	@Override
	protected void init() {
		super.init();

		IResourceSettings resourceSettings = getResourceSettings();
                resourceSettings.addResourceFolder("pages");

	}
} 
```

Download It – [Wicket1.4-resource-loading.zip](http://web.archive.org/web/20190306164340/http://www.mkyong.com/wp-content/uploads/2009/02/Wicket1.4-resource-loading.zip) (8KB)**Maven ways**
Alternatively, you can control where the HTML files is loaded via Maven resource tag like this :

*文件:pom.xml*

```java
 <project ...>
	<build>
		<finalName>WicketExamples</finalName>

		<resources>
			<resource>
				<directory>src/main/resources</directory>
			</resource>
			<resource>
				<directory>src/main/webapp/pages</directory>
			</resource>
		</resources>

	</build>
</project> 
```

以 Maven 的方式，您不需要覆盖 WebApplication **init()** 方法。

## 参考

1.  [Wicket——控制从](http://web.archive.org/web/20190306164340/https://cwiki.apache.org/WICKET/control-where-html-files-are-loaded-from.html#ControlwhereHTMLfilesareloadedfrom-InWicket1.4)加载 HTML 文件的位置

[wicket](http://web.archive.org/web/20190306164340/http://www.mkyong.com/tag/wicket/)







