# `JarRunner`类

> 原文：[`docs.oracle.com/javase/tutorial/deployment/jar/jarrunner.html`](https://docs.oracle.com/javase/tutorial/deployment/jar/jarrunner.html)

`JarRunner`应用程序是以这种形式启动的：

```java
java JarRunner *url [arguments]*

```

在前一节中，我们已经看到`JarClassLoader`如何能够从给定的 URL 标识和加载 JAR 捆绑应用程序的主类。因此，要完成`JarRunner`应用程序，我们需要能够从命令行获取 URL 和任何参数，并将它们传递给`JarClassLoader`的实例。这些任务属于`JarRunner`类，是`JarRunner`应用程序的入口点。

它首先通过命令行指定的 URL 创建一个`java.net.URL`对象：

```java
public static void main(String[] args) {
    if (args.length < 1) {
        usage();
    }
    URL url = null;
    try {
        url = new URL(args[0]);
    } catch (MalformedURLException e) {
        fatal("Invalid URL: " + args[0]);
    }

```

如果`args.length < 1`，这意味着在命令行中没有指定 URL，因此会打印出使用消息。如果第一个命令行参数是一个有效的 URL，则会创建一个新的`URL`对象来表示它。

接下来，`JarRunner`创建了一个新的`JarClassLoader`实例，并将在命令行中指定的 URL 传递给构造函数：

```java
JarClassLoader cl = new JarClassLoader(url);

```

正如我们在前一节中看到的，通过`JarClassLoader`，`JarRunner`利用 JAR 处理 API。

传递给`JarClassLoader`构造函数的 URL 是您想要运行的 JAR 捆绑应用程序的 URL。`JarRunner`接下来调用类加载器的`getMainClassName`方法来识别应用程序的入口点类：

```java
String name = null;
try {
    name = cl.getMainClassName();
} catch (IOException e) {
    System.err.println("I/O error while loading JAR file:");
    e.printStackTrace();
    System.exit(1);
}
if (name == null) {
    fatal("Specified jar file does not contain a 'Main-Class'" +
          " manifest attribute");
}

```

关键语句已用粗体标出。其他语句用于错误处理。

一旦`JarRunner`确定了应用程序的入口点类，只剩下两个步骤：将任何参数传递给应用程序并实际启动应用程序。`JarRunner`使用以下代码执行这些步骤：

```java
// Get arguments for the application
String[] newArgs = new String[args.length - 1];
System.arraycopy(args, 1, newArgs, 0, newArgs.length);
// Invoke application's main class
try {
    cl.invokeClass(name, newArgs);
} catch (ClassNotFoundException e) {
    fatal("Class not found: " + name);
} catch (NoSuchMethodException e) {
    fatal("Class does not define a 'main' method: " + name);
} catch (InvocationTargetException e) {
    e.getTargetException().printStackTrace();
    System.exit(1);
}

```

回想一下，第一个命令行参数是 JAR 捆绑应用程序的 URL。因此，要传递给该应用程序的任何参数都在`args`数组中的元素`1`及以后。`JarRunner`获取这些元素，并创建一个名为`newArgs`的新数组传递给应用程序（上面的粗体行）。然后，`JarRunner`将入口点的类名和新参数列表传递给`JarClassLoader`的`invokeClass`方法。正如我们在前一节中看到的，`invokeClass`将加载应用程序的入口点类，传递任何参数，并启动应用程序。
