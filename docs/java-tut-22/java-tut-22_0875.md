# 高分开发者（克里斯）的步骤

> 原文：[`docs.oracle.com/javase/tutorial/security/userperm/chris.html`](https://docs.oracle.com/javase/tutorial/security/userperm/chris.html)

克里斯在创建`HighScore`和`HighScorePermission`类之后将采取的步骤是：

## 编译这些类

```java
javac HighScore*.java -d .

```

## 将类文件放入一个 JAR 文件中

```java
jar cvf hs.jar com/scoredev/scores/HighScore*.class

```

## 创建用于签名的密钥库和密钥

```java
keytool -genkey -keystore chris.keystore -alias signJars

```

指定密码和显著名称信息

## 签署 JAR 文件

```java
jarsigner -keystore chris.keystore hs.jar signJars

```

## 导出公钥证书

```java
keytool -export -keystore chris.keystore
    -alias signJars -file Chris.cer

```

## 提供游戏开发人员和用户所需的文件和信息

也就是说，提供它们

+   签名的 JAR 文件`hs.jar`，

+   公钥证书文件`Chris.cer`，

+   `HighScore`和`HighScorePermission`类在策略文件中必须被授予的权限信息，以便能够正常工作。对于这一点，克里斯可以提供所需的确切授权条目。
