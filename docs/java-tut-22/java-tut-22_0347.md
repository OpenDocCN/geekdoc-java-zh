# `JarClassLoader`类

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/jarclassloader.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/jarclassloader.html)

`JarClassLoader`类扩展了`java.net.URLClassLoader`。顾名思义，`URLClassLoader`被设计用于加载通过搜索一组 URL 访问的类和资源。这些 URL 可以是目录或 JAR 文件。

除了继承`URLClassLoader`，`JarClassLoader`还利用了另外两个新的与 JAR 相关的 API 特性，即`java.util.jar`包和`java.net.JarURLConnection`类。在本节中，我们将详细查看`JarClassLoader`的构造函数和两个方法。

## `JarClassLoader`构造函数

构造函数以`java.net.URL`的实例作为参数。传递给这个构造函数的 URL 将在`JarClassLoader`的其他地方使用，以找到要加载类的 JAR 文件。

```java
public JarClassLoader(URL url) {
    super(new URL[] { url });
    this.url = url;
}

```

`URL`对象被传递给父类`URLClassLoader`的构造函数，该构造函数接受一个`URL[]`数组作为参数，而不是单个`URL`实例。

## `getMainClassName`方法

一旦使用 JAR 捆绑应用程序的 URL 构造了一个`JarClassLoader`对象，它将需要一种方法来确定 JAR 文件中哪个类是应用程序的入口点。这就是`getMainClassName`方法的工作：

```java
public String getMainClassName() throws IOException {
    URL u = new URL("jar", "", url + "!/");
    JarURLConnection uc = (JarURLConnection)u.openConnection();
    Attributes attr = uc.getMainAttributes();
    return attr != null
                   ? attr.getValue(Attributes.Name.MAIN_CLASS)
                   : null;
}

```

你可能还记得之前的一节课中提到过，JAR 捆绑应用程序的入口点是由 JAR 文件清单的`Main-Class`头部指定的。为了理解`getMainClassName`如何访问`Main-Class`头部的值，让我们详细看一下这个方法，特别关注它使用的新的 JAR 处理特性：

## `JarURLConnection`类和 JAR URL

`getMainClassName`方法使用了`java.net.JarURLConnection`类指定的 JAR URL 格式。JAR 文件的 URL 语法如下例所示：

```java
jar:http://www.example.com/jarfile.jar!/

```

终止的`!/`分隔符表示 URL 指向整个 JAR 文件。分隔符后面的任何内容都指向特定的 JAR 文件内容，就像这个例子中一样：

```java
jar:http://www.example.com/jarfile.jar!/mypackage/myclass.class

```

`getMainClassName`方法中的第一行是：

```java
URL u = new URL("jar", "", url + "!/");

```

这个语句构造了一个新的`URL`对象，表示一个 JAR URL，将`JarClassLoader`实例创建时使用的 URL 后面添加了`!/`分隔符。

## `java.net.JarURLConnection`类

这个类表示应用程序和 JAR 文件之间的通信链接。它有用于访问 JAR 文件清单的方法。`getMainClassName`的第二行是：

```java
JarURLConnection uc = (JarURLConnection)u.openConnection();

```

在这个语句中，第一行创建的`URL`实例打开了一个`URLConnection`。然后将`URLConnection`实例转换为`JarURLConnection`，以便利用`JarURLConnection`的 JAR 处理特性。

## 获取清单属性：`java.util.jar.Attributes`

通过打开到 JAR 文件的`JarURLConnection`，您可以使用`JarURLConnection`的`getMainAttributes`方法访问 JAR 文件 MANIFEST 中的头信息。此方法返回一个`java.util.jar.Attributes`的实例，该类将 JAR 文件 MANIFEST 中的头名称与其关联的字符串值进行映射。`getMainClassName`中的第三行创建了一个`Attributes`对象：

```java
Attributes attr = uc.getMainAttributes();

```

要获取 MANIFEST 的`Main-Class`头的值，`getMainClassName`的第四行调用了`Attributes.getValue`方法：

```java
return attr != null
               ? attr.getValue(Attributes.Name.MAIN_CLASS)
               : null;

```

方法的参数`Attributes.Name.MAIN_CLASS`指定了您想要的`Main-Class`头的值。（`Attributes.Name`类还提供了静态字段，如`MANIFEST_VERSION`、`CLASS_PATH`和`SEALED`，用于指定其他标准 MANIFEST 头。）

## invokeClass 方法

我们已经看到`JarURLClassLoader`如何在打包为 JAR 的应用程序中识别主类。要考虑的最后一个方法是`JarURLClassLoader.invokeClass`，它使得可以调用主类来启动打包为 JAR 的应用程序：

```java
public void invokeClass(String name, String[] args)
    throws ClassNotFoundException,
           NoSuchMethodException,
           InvocationTargetException
{
    Class c = loadClass(name);
    Method m = c.getMethod("main", new Class[] { args.getClass() });
    m.setAccessible(true);
    int mods = m.getModifiers();
    if (m.getReturnType() != void.class || !Modifier.isStatic(mods) ||
        !Modifier.isPublic(mods)) {
        throw new NoSuchMethodException("main");
    }
    try {
        m.invoke(null, new Object[] { args });
    } catch (IllegalAccessException e) {
        // This should not happen, as we have disabled access checks
    }
}

```

`invokeClass`方法接受两个参数：应用程序入口点类的名称和要传递给入口点类的`main`方法的字符串参数数组。首先，加载主类：

```java
Class c = loadClass(name);

```

`loadClass`方法是从`java.lang.ClassLoader`继承的。

一旦主类加载完成，就会使用`java.lang.reflect`包的反射 API 将参数传递给类并启动它。您可以参考反射 API 上的教程来复习反射。
