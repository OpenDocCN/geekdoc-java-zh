# 环境变量

> 原文：[`docs.oracle.com/javase/tutorial/essential/environment/env.html`](https://docs.oracle.com/javase/tutorial/essential/environment/env.html)

许多操作系统使用*环境变量*将配置信息传递给应用程序。与 Java 平台中的属性类似，环境变量是键/值对，其中键和值都是字符串。设置和使用环境变量的约定在操作系统之间以及命令行解释器之间有所不同。要了解如何在您的系统上将环境变量传递给应用程序，请参考系统文档。

## 查询环境变量

在 Java 平台上，应用程序使用[`System.getenv`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#getenv--)来检索环境变量的值。没有参数时，`getenv`返回一个只读的`java.util.Map`实例，其中映射键是环境变量名称，映射值是环境变量值。这在``EnvMap``示例中有所展示：

```java

import java.util.Map;

public class EnvMap {
    public static void main (String[] args) {
        Map<String, String> env = System.getenv();
        for (String envName : env.keySet()) {
            System.out.format("%s=%s%n",
                              envName,
                              env.get(envName));
        }
    }
}

```

使用`String`参数，`getenv`返回指定变量的值。如果未定义变量，则`getenv`返回`null`。``Env``示例以这种方式使用`getenv`来查询在命令行上指定的特定环境变量：

```java

public class Env {
    public static void main (String[] args) {
        for (String env: args) {
            String value = System.getenv(env);
            if (value != null) {
                System.out.format("%s=%s%n",
                                  env, value);
            } else {
                System.out.format("%s is"
                    + " not assigned.%n", env);
            }
        }
    }
}

```

## 将环境变量传递给新进程

当 Java 应用程序使用[`ProcessBuilder`](https://docs.oracle.com/javase/8/docs/api/java/lang/ProcessBuilder.html)对象创建新进程时，传递给新进程的默认环境变量集合与提供给应用程序的虚拟机进程的集合相同。应用程序可以使用`ProcessBuilder.environment`更改此集合。

## 平台依赖性问题

不同系统上实现环境变量的方式之间存在许多微妙的差异。例如，Windows 在环境变量名称中忽略大小写，而 UNIX 则不会。环境变量的使用方式也各不相同。例如，Windows 在名为`USERNAME`的环境变量中提供用户名，而 UNIX 实现可能在`USER`、`LOGNAME`或两者中提供用户名。

为了最大化可移植性，在系统属性中提供相同值时，永远不要引用环境变量。例如，如果操作系统提供用户名，则始终可以在系统属性`user.name`中找到。
