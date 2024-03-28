# 示例游戏开发者（Terry）的步骤

> 原文：[`docs.oracle.com/javase/tutorial/security/userperm/terry.html`](https://docs.oracle.com/javase/tutorial/security/userperm/terry.html)

Terry 创建一个调用 `HighScore` 的 `getHighScore` 和 `setHighScore` 方法来获取和设置用户高分的游戏（`ExampleGame`）后，Terry 需要采取的步骤是：

## 编译游戏类

```java
javac ExampleGame.java -classpath hs.jar -d .

```

## 将其类文件放入一个 JAR 文件中

```java
jar cvf terry.jar com/gamedev/games/ExampleGame.class

```

## 创建用于签名的密钥库和密钥

```java
keytool -genkey -keystore terry.keystore -alias signTJars

```

为密码和区分名称信息指定任何你想要的内容。

## 签署 JAR 文件

```java
jarsigner -keystore terry.keystore terry.jar signTJars

```

## 导出公钥证书

```java
keytool -export -keystore terry.keystore
    -alias signTJars -file Terry.cer

```

## 为用户提供所需的文件和信息

也就是说，向他们提供

+   签名的 JAR 文件 `terry.jar,`

+   公钥证书文件 `Terry.cer`, 和

+   `ExampleGame` 类所需权限的信息。对于这一点，Terry 可能会提供所需的确切授权条目。

游戏用户还需要来自 Chris 的文件和信息。为了方便他们，Terry 可能会将这些信息转发给他们：

+   签名的 JAR 文件 `hs.jar`,

+   公钥证书文件 `Chris.cer`, 和

+   有关 `HighScore` 和 `HighScorePermission` 类在策略文件中必须被授予的权限的信息，以便其正常工作。这可能是所需的确切授权条目。
