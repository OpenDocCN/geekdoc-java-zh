# 教训：常见问题（及其解决方法）

> 原文：[`docs.oracle.com/javase/tutorial/getStarted/problems/index.html`](https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html)

## 编译器问题

**Microsoft Windows 系统上的常见错误消息**

`**'javac' is not recognized as an internal or external command, operable program or batch file**`

如果你收到这个错误，Windows 找不到编译器(`javac`)。

下面是告诉 Windows 如何找到`javac`的一种方法。假设你将 JDK 安装在`C:\jdk1.8.0`中。在提示符下，你可以输入以下命令并按回车：

```java
C:\jdk1.8.0\bin\javac HelloWorldApp.java

```

如果选择此选项，每次编译或运行程序时，都需要在`C:\jdk1.8.0\bin\`之前加上`javac`和`java`命令。为了避免这种额外的输入，可以参考 JDK 8 安装说明中的[更新 PATH 变量](https://docs.oracle.com/javase/8/docs/technotes/guides/install/windows_jdk_install.html#BABGDJFH)部分。

`**Class names, 'HelloWorldApp', are only accepted if annotation processing is explicitly requested**`

如果你收到这个错误，那么在编译程序时忘记包含`.java`后缀。记住，命令是`javac HelloWorldApp.java`而不是`javac HelloWorldApp`。

**UNIX 系统上的常见错误消息**

`javac: Command not found`

如果你收到这个错误，UNIX 找不到编译器`javac`。

下面是告诉 UNIX 如何找到`javac`的一种方法。假设你将 JDK 安装在`/usr/local/jdk1.8.0`中。在提示符下，你可以输入以下命令并按回车：

```java
/usr/local/jdk1.8.0/javac HelloWorldApp.java

```

**注意：** 如果选择此选项，每次编译或运行程序时，都需要在`javac`和`java`命令之前加上`/usr/local/jdk1.8.0/`。为了避免这种额外的输入，你可以将这些信息添加到你的 PATH 变量中。具体操作步骤将取决于你当前正在运行的 shell。

`**Class names, 'HelloWorldApp', are only accepted if annotation processing is explicitly requested**`

如果你收到这个错误，那么在编译程序时忘记包含`.java`后缀。记住，命令是`javac HelloWorldApp.java`而不是`javac HelloWorldApp`。

**语法错误（所有平台）**

如果你在程序的某个部分打错了字，编译器可能会发出*语法*错误。错误信息通常显示错误的类型、检测到错误的行号、该行的代码以及错误在代码中的位置。以下是由于在语句末尾省略分号(`;`)而引起的错误：

```java
Testing.java:8: error: ';' expected
            count++
                   ^
1 error

```

如果你看到任何编译器错误，那么你的程序没有成功编译，编译器没有创建`.class`文件。仔细检查程序，修复你发现的任何错误，然后再试一次。

**语义错误**

除了验证你的程序在语法上是否正确之外，编译器还会检查其他基本的正确性。例如，每次使用未初始化的变量时，编译器都会警告你：

```java
Testing.java:8: error: variable count might not have been initialized
            count++;
            ^
Testing.java:9: error: variable count might not have been initialized
        System.out.println("Input has " + count + " chars.");
                                          ^
2 errors

```

再次，你的程序没有成功编译，编译器没有创建`.class`文件。修复错误并重试。

## 运行时问题

Microsoft Windows 系统上的错误消息

`主线程中的异常"main" java.lang.NoClassDefFoundError: HelloWorldApp`

如果你收到这个错误，`java`找不到你的字节码文件`HelloWorldApp.class`。

`java`尝试查找你的`.class`文件的一个地方是你当前的目录。所以如果你的`.class`文件在`C:\java`中，你应该将当前目录更改为那个目录。要更改你的目录，请在提示符下输入以下命令并按回车键：

```java
cd c:\java

```

提示符应该变成`C:\java>`。如果你在提示符下输入`dir`，你应该看到你的`.java`和`.class`文件。现在再次输入`java HelloWorldApp`。

如果你仍然遇到问题，你可能需要更改你的 CLASSPATH 变量。要查看是否需要这样做，请尝试使用以下命令清除类路径。

```java
set CLASSPATH=

```

现在再次输入`java HelloWorldApp`。如果程序现在可以运行，那么你需要更改你的 CLASSPATH 变量。要设置这个变量，请参考 JDK 8 安装说明中的[更新 PATH 变量](https://docs.oracle.com/javase/8/docs/technotes/guides/install/windows_jdk_install.html#BABGDJFH)部分。CLASSPATH 变量的设置方式相同。

`**找不到或加载主类 HelloWorldApp.class**`

初学者程序员常犯的一个常见错误是尝试在编译器创建的`.class`文件上运行`java`启动器。例如，如果你尝试用`java HelloWorldApp.class`而不是`java HelloWorldApp`来运行你的程序，你会得到这个错误。记住，参数是你想要使用的*类名*，*不是*文件名。

**`主线程中的异常"main" java.lang.NoSuchMethodError: main`**

Java 虚拟机要求你执行的类具有一个`main`方法，用于开始执行你的应用程序。深入了解"Hello World!"应用程序详细讨论了`main`方法。

**UNIX 系统上的错误消息**

`主线程中的异常"main" java.lang.NoClassDefFoundError: HelloWorldApp`

如果你收到这个错误，`java`找不到你的字节码文件`HelloWorldApp.class`。

`java`尝试查找你的字节码文件的一个地方是你当前的目录。所以，例如，如果你的字节码文件在`/home/jdoe/java`中，你应该将当前目录更改为那个目录。要更改你的目录，请在提示符下输入以下命令并按回车键：

```java
cd /home/jdoe/java

```

如果你在提示符下输入`pwd`，你应该看到`/home/jdoe/java`。如果你在提示符下输入`ls`，你应该看到你的`.java`和`.class`文件。现在再次输入`java HelloWorldApp`。

如果你仍然遇到问题，你可能需要更改你的 CLASSPATH 环境变量。要查看是否需要这样做，请尝试使用以下命令清除类路径。

```java
unset CLASSPATH

```

现在再次输入`java HelloWorldApp`。如果程序现在可以运行，你需要像上面的 PATH 变量一样更改你的 CLASSPATH 变量。

`**主线程中的异常"main"java.lang.NoClassDefFoundError: HelloWorldApp/class**`

初学者程序员常犯的一个错误是尝试在编译器创建的`.class`文件上运行`java`启动器。例如，如果你尝试用`java HelloWorldApp.class`而不是`java HelloWorldApp`来运行你的程序，你会得到这个错误。记住，参数是你想要使用的*类名*，而不是文件名。

**`主线程中的异常"main"java.lang.NoSuchMethodError: main`**

Java 虚拟机要求你执行的类具有一个`main`方法，用于开始执行你的应用程序。深入了解"Hello World!"应用程序详细讨论了`main`方法。

**小程序或 Java Web Start 应用程序被阻止**

如果你通过浏览器运行一个应用程序并收到安全警告说应用程序被阻止，请检查以下项目：

+   验证 JAR 文件清单中的属性是否针对应用程序运行的环境正确设置。权限属性是必需的。在 NetBeans 项目中，你可以通过在 NetBeans IDE 的文件选项卡中展开项目文件夹并双击`manifest.mf`来打开清单文件。

+   验证应用程序是否由有效证书签名，并且证书位于签名者 CA 密钥库中。

+   如果你正在运行一个本地小程序，建立一个用于测试的 Web 服务器。你也可以将你的应用程序添加到例外站点列表中，在 Java 控制面板的安全选项卡中管理。
