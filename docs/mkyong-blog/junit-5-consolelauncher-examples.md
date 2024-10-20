# JUnit 5 控制台启动器示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/junit5/junit-5-consolelauncher-examples/>

本文向您展示了如何使用 JUnit 5 `ConsoleLauncher`从命令行运行测试。

测试对象

*   JUnit 5.5.2
*   JUnit-平台-控制台-独立版 1.5.2

## 1.下载 JAR

要从命令行运行测试，我们可以手动从 Maven central repository 下载[JUnit-platform-console-standalone . jar](http://web.archive.org/web/20221225035521/https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/)。

这个例子使用的是版本`1.5.2`。

https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.5.2/![run tests from console](img/d5016bb1312c674665dbae80cbc7f715.png)

## 2.习惯

通常，测试类位于以下类路径中:

*   `build/classes/java/test`
*   `target/test-classes`

2.1 从此类路径运行所有测试`build/classes/java/test`

Terminal

```java
 $ java -jar junit-platform-console-standalone-1.5.2.jar --classpath build/classes/java/test --scan-classpath

$ java -jar junit-platform-console-standalone-1.5.2.jar -cp build/classes/java/test --scan-classpath 
```

2.2 使用类名运行指定的测试。

Terminal

```java
 $ java -jar junit-platform-console-standalone-1.5.2.jar 
	-cp 'build/classes/java/test' 
	-c com.mkyong.order.MethodOrderTest

$ java -jar junit-platform-console-standalone-1.5.2.jar 
	-cp 'build/classes/java/test' 
	--select-class com.mkyong.order.MethodOrderTest 
```

2.3 运行包中的所有测试。

Terminal

```java
 $ java -jar junit-platform-console-standalone-1.5.2.jar 
	-cp 'build/classes/java/test' 
	--select-package com.mkyong.order 
```

2.4 运行包中的所有测试，通过正则表达式模式过滤测试的类名。

Terminal

```java
 $ java -jar junit-platform-console-standalone-1.5.2.jar 
	-cp 'build/classes/java/test' 
	--select-package com.mkyong.order 
	--include-classname='.*Count.*' 
```

2.5 输出样本。

Terminal

```java
 $ java -jar junit-platform-console-standalone-1.5.2.jar 
	-cp 'build/classes/java/test' 
	--select-package com.mkyong.order 
	--include-classname='.*'

Thanks for using JUnit! Support its development at https://junit.org/sponsoring

+-- JUnit Jupiter [OK]
| +-- MethodRandomTest [OK]
| | +-- testA() [OK]
| | +-- testZ() [OK]
| | +-- testY() [OK]
| | +-- testE() [OK]
| | '-- testB() [OK]
| +-- MethodParameterCountTest [OK]
| | +-- Parameter Count : 3 [OK]
| | | +-- 1 ==> fruit='apple', qty=1, price=1.99 [OK]
| | | '-- 2 ==> fruit='banana', qty=2, price=2.99 [OK]
| | +-- Parameter Count : 2 [OK]
| | | +-- 1 ==> fruit='apple', qty=1 [OK]
| | | '-- 2 ==> fruit='banana', qty=2 [OK]
| | '-- Parameter Count : 1 [OK]
| |   +-- 1 ==> ints=1 [OK]
| |   +-- 2 ==> ints=2 [OK]
| |   '-- 3 ==> ints=3 [OK]
| +-- MethodAlphanumericTest [OK]
| | +-- testA() [OK]
| | +-- testB() [OK]
| | +-- testE() [OK]
| | +-- testY() [OK]
| | '-- testZ() [OK]
| '-- MethodOrderTest [OK]
|   +-- test2() [OK]
|   +-- test3() [OK]
|   +-- test1() [OK]
|   +-- test0() [OK]
|   '-- test4() [OK]
'-- JUnit Vintage [OK]

Test run finished after 109 ms
[         9 containers found      ]
[         0 containers skipped    ]
[         9 containers started    ]
[         0 containers aborted    ]
[         9 containers successful ]
[         0 containers failed     ]
[        22 tests found           ]
[         0 tests skipped         ]
[        22 tests started         ]
[         0 tests aborted         ]
[        22 tests successful      ]
[         0 tests failed          ] 
```

*附:请参考 JUnit 5 官方指南，了解所有[控制台启动器选项](http://web.archive.org/web/20221225035521/https://junit.org/junit5/docs/current/user-guide/#running-tests-console-launcher-options)*

## 3.Windows 操作系统

3.1 如果我们想在经典命令提示符下运行上述命令，请用双引号替换单引号，例如:

Command Prompt – Windows

```java
 $ java -jar junit-platform-console-standalone-1.5.2.jar 
	-cp "build/classes/java/test"
	-c com.mkyong.order.MethodOrderTest 
```

3.2 传递一个`--disable-ansi-colors`选项来禁用输出中的 ANSI 颜色。命令提示符不支持此功能。

Command Prompt – Windows

```java
 $ java -jar junit-platform-console-standalone-1.5.2.jar 
	-cp "build/classes/java/test"
	-c com.mkyong.order.MethodOrderTest
	--disable-ansi-colors 
```

## 下载源代码

$ git clone [https://github.com/mkyong/junit-examples](http://web.archive.org/web/20221225035521/https://github.com/mkyong/junit-examples)

# 参考

*   [JUnit 5 控制台启动器](http://web.archive.org/web/20221225035521/https://junit.org/junit5/docs/current/user-guide/#running-tests-console-launcher)
*   [运行 JAR-Packaged](http://web.archive.org/web/20221225035521/https://docs.oracle.com/javase/tutorial/deployment/jar/run.html)

<input type="hidden" id="mkyong-current-postId" value="15256">