# 高分类

> 原文：[`docs.oracle.com/javase/tutorial/security/userperm/highscore.html`](https://docs.oracle.com/javase/tutorial/security/userperm/highscore.html)

`HighScore`类存储并保护用户在`ExampleGame`（以及调用它的任何其他游戏）中的高分值的访问。为简单起见，该类将高分值保存到名为`.highscore`的文件中，该文件位于用户的主目录中。但是，在允许`ExampleGame`检索或更新用户的高分值之前，该类会检查用户是否已在其安全策略文件中授予`ExampleGame`访问高分的权限。

## 检查`ExampleGame`是否具有`HighScorePermission`

要检查`ExampleGame`是否具有访问用户高分值的权限，`HighScore`类必须：

1.  调用`System.getSecurityManager()`以获取当前安装的安全管理器。

1.  如果结果不为空（也就是说，存在一个安全管理器，而不是调用者是一个无限制的应用程序），那么

    1.  构造一个`HighScorePermission`对象，并

    1.  调用安全管理器的`checkPermission`方法，并传递新构造的`HighScorePermission`对象。

这是代码：

```java
SecurityManager sm = System.getSecurityManager();
if (sm != null) {
    sm.checkPermission(
        new HighScorePermission(gameName));
}

```

`checkPermission`方法本质上是询问安全管理器是否`ExampleGame`具有指定的`HighScorePermission`。换句话说，它询问安全管理器是否`ExampleGame`有权限更新指定游戏（`ExampleGame`）的用户高分值。底层安全框架将查阅用户的安全策略，以查看`ExampleGame`是否确实具有此权限。

## 高分代码

`这里`是`HighScore`类的完整源代码。

注意：`doPrivileged`方法调用用于使`HighScore`能够临时访问对其可用但对调用它的代码（`ExampleGame`）不可用的资源。例如，预期策略文件将授予`HighScore`访问用户主目录中的`.highscore`文件的权限，但不会授予这些权限给游戏，如`ExampleGame`。
