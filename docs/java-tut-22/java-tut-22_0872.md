# HighScorePermission 类

> 原文：[`docs.oracle.com/javase/tutorial/security/userperm/perm.html`](https://docs.oracle.com/javase/tutorial/security/userperm/perm.html)

`HighScorePermission` 类定义了 `ExampleGame` 需要更新用户高分的权限。

所有权限类都应该从 `java.security.Permission` 或 `java.security.BasicPermission` 中的一个子类化。两者之间的基本区别在于，`java.security.Permission` 定义了需要名称和操作的更复杂的权限。例如，`java.io.FilePermission` 扩展自 `java.security.Permission`，并需要一个名称（文件名）以及该文件允许的操作（读/写/删除）。

相比之下，`java.security.BasicPermission` 定义了只需要名称的更简单的权限。例如，`java.lang.RuntimePermission` 扩展自 `java.security.BasicPermission`，只需要一个名称（如 "exitVM"），允许程序退出 Java 虚拟机。

我们的 `HighScorePermission` 是一个简单的权限，因此可以从 `java.security.BasicPermission` 扩展。

通常，`BasicPermission` 类中的方法实现本身不需要被其子类重写。这就是我们的 `HighScorePermission` 的情况，所以我们只需要实现构造函数，它们只是调用超类的构造函数，如`以下`所示：

```java

package com.scoredev.scores;

import java.security.*;

public final class HighScorePermission extends BasicPermission {

    public HighScorePermission(String name)
    {
	super(name);
    }

    // note that actions is ignored and not used,
    // but this constructor is still needed
    public HighScorePermission(String name, String actions) 
    {
	super(name, actions);
    }
}

```
