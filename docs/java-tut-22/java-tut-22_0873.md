# 一个示例策略文件

> 原文：[`docs.oracle.com/javase/tutorial/security/userperm/policy.html`](https://docs.oracle.com/javase/tutorial/security/userperm/policy.html)

以下是一个完整的策略文件，供用户运行`ExampleGame`使用。

这里不描述策略文件的语法；如果您感兴趣，请参阅[默认策略实现和策略文件语法](https://docs.oracle.com/javase/8/docs/technotes/guides/security/PolicyFiles.html)页面。

你不需要了解语法；你可以随时使用策略工具创建策略文件，如创建策略文件，控制应用程序的快速导览，以及签署代码并授予权限课程中所示。

以下是示例策略文件，后面是各个条目的描述。假设

+   策略文件位于 Kim 的计算机上，Kim 的密钥库命名为`kim.keystore`。

+   `ExampleGame`已由游戏创建者 Terry 的私钥签名，相应的公钥在别名为`"terry"`的密钥库条目中。

+   `HighScore`和`HighScorePermissions`类是由实现它们的人（Chris）的私钥签名的，相应的公钥在别名为`"chris"`的密钥库条目中。

这是策略文件：`kim.policy`

```java
keystore "kim.keystore";

// Here is the permission ExampleGame needs.
// It grants code signed by "terry" the
// HighScorePermission, if the
// HighScorePermission was signed by "chris"
grant SignedBy "terry" {
  permission
    com.scoredev.scores.HighScorePermission
      "ExampleGame", signedBy "chris";
};

// Here is the set of permissions the HighScore
// class needs:
grant SignedBy "chris" {
  // The HighScore class needs permission to read
  // "user.home" to find the location of the
  // highscore file

  permission java.util.PropertyPermission
    "user.home", "read";

  // It needs permission to read and write the
  // high score file itself

  permission java.io.FilePermission
      "${user.home}${/}.highscore", "read,write";

  // It needs to get granted its own permission,
  // so it can call checkPermission
  // to see if its caller has permission.
  // Only grant it the permission
  // if the permission itself was signed by
  // "chris"

  permission
    com.scoredev.scores.HighScorePermission 
      "*", signedBy "chris";
};

```

## 密钥库条目

*密钥库*是密钥和证书的存储库，用于查找策略文件中指定的签名者的公钥（在本例中为`"terry"`和`"chris"`）。

`keytool`实用程序用于创建和管理密钥库。

对于本课程，假设 Kim 想玩`ExampleGame`。如果 Kim 的密钥库命名为`kim.keystore`，那么 Kim 的策略文件需要在开头加上以下行：

```java
keystore "kim.keystore";

```

## ExampleGame 条目

策略文件条目指定了特定*代码源*的一个或多个权限 - 来自特定位置（URL）的代码，或者由特定实体签名的代码，或两者兼有。

我们的策略文件需要为每个游戏添加一个条目，为该游戏的创建者签名的代码授予一个名为`HighScorePermission`的权限，其名称为游戏名称。该权限允许游戏调用`HighScore`方法来获取或更新该特定游戏用户的最高分值。

为`ExampleGame`所需的条目是：

```java
grant SignedBy "terry" {
    permission
        com.scoredev.scores.HighScorePermission 
            "ExampleGame", signedBy "chris";
};

```

要求`ExampleGame`由`"terry"`签名使 Kim 知道该游戏是 Terry 开发的实际游戏。为了使其工作，Kim 必须已经将 Terry 的公钥证书存储到`kim.keystore`中，别名为`"terry"`。

注意，`HighScorePermission`需要由实际实现该权限的`"chris"`签名，以确保`ExampleGame`被授予由`"chris"`实现的实际权限，而不是其他人。与之前一样，为了使其工作，Kim 必须已经将 Chris 的公钥证书存储到`kim.keystore`中，别名为`"chris"`。

## 最高分条目

策略文件中的最后一个条目授予`HighScore` 类权限。更具体地说，它授予由`"chris"`签名的代码权限，他创建并签署了这个类。要求类由`"chris"`签名确保当`ExampleGame` 调用这个类来更新用户的高分时，`ExampleGame` 确切知道它正在使用由`"chris"`实现的原始类。

要更新调用它的任何游戏的用户高分值，`HighScore` 类需要三个权限：

### 1\. 读取`"user.home"`属性值的权限。

`HighScore` 类将用户的高分值存储在用户主目录中的`.highscore`文件中。因此，这个类需要一个`java.util.PropertyPermission`，允许它读取`"user.home"`属性值，以确定用户主目录的确切位置：

```java
permission java.util.PropertyPermission 
    "user.home", "read";

```

### 2\. 读写高分文件本身的权限。

这个权限是为了让`HighScore` 的`getHighScore` 和 `setHighScore` 方法可以访问用户的`.highscore`文件，分别获取或设置当前游戏的当前高分。

这是所需的权限：

```java
permission java.io.FilePermission
    "${user.home}${/}.highscore", "read,write";

```

注意：`${propName}` 表示属性的值。因此，`${user.home}` 将被`"user.home"`属性的值替换。`${/}` 表示文件分隔符的平台无关方式。

### 3\. 所有 HighScorePermissions（即任何名称的 HighScorePermissions）。

这个权限是为了确保`HighScore` 检查调用游戏是否被授予了一个名为游戏名称的`HighScorePermission`。也就是说，`HighScore` 类必须*同样*被授予权限，因为权限检查要求堆栈上的所有代码都具有指定的权限。

这是所需的权限：

```java
permission com.scoredev.scores.HighScorePermission
    "*", signedBy "chris";

```

与以前一样，`HighScorePermission` 本身需要由实际实现权限的`"chris"`签名。
