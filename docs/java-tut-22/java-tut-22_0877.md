# 运行 ExampleGame（Kim）的用户步骤

> 原文：[`docs.oracle.com/javase/tutorial/security/userperm/kim.html`](https://docs.oracle.com/javase/tutorial/security/userperm/kim.html)

用户（比如 Kim）需要执行的步骤包括：

## 将证书导入为受信任的证书

```java
keytool -import -alias chris -file Chris.cer -keystore kim.keystore
keytool -import -alias terry -file Terry.cer -keystore kim.keystore

```

## 设置具有所需权限的策略文件

这里是完整的`kim.policy`策略文件，如 A Sample Policy File 中所述。

## 运行 ExampleGame

设置高分：

```java
java -Djava.security.manager 
    -Djava.security.policy=kim.policy
    -classpath hs.jar;terry.jar
    com.gamedev.games.ExampleGame set 456

```

获取高分：

```java
java -Djava.security.manager
    -Djava.security.policy=kim.policy
    -classpath hs.jar;terry.jar
    com.gamedev.games.ExampleGame get

```

注意：

+   如果不指定`-Djava.security.manager`，应用程序将无限制地运行（策略文件和权限不会被检查）。

+   `-Djava.security.policy=kim.policy`指定了策略文件的位置。注意：还有其他指定策略文件的方法。例如，你可以在安全属性文件中添加一个条目，指定包含`kim.policy`，如在查看策略文件效果课程末尾讨论的那样。

+   `-classpath hs.jar;terry.jar`指定了包含所需类文件的 JAR 文件。对于 Windows，使用分号（";"）分隔 JAR 文件；对于 UNIX，使用冒号（":"）。

+   策略文件`kim.policy`指定了密钥库`kim.keystore`。由于未提供密钥库的绝对 URL 位置，因此假定密钥库与策略文件位于同一目录中。
