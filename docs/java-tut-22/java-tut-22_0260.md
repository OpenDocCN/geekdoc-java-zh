# 里程碑方法

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/appletMethods.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/appletMethods.html)

[`Applet`](https://docs.oracle.com/javase/8/docs/api/java/applet/Applet.html)类为小程序执行提供了一个框架，定义了系统在里程碑发生时调用的方法。里程碑是小程序生命周期中的重要事件。大多数小程序会覆盖其中一些或全部方法以适当地响应里程碑。

## `init` 方法

`init`方法适用于不需要太长时间的一次性初始化。`init`方法通常包含通常放在构造函数中的代码。小程序通常没有构造函数的原因是在调用其`init`方法之前不能保证其具有完整的环境。保持`init`方法简短，以便您的小程序可以快速加载。

## `start` 方法

每个在初始化后执行任务（除了直接响应用户操作）的小程序必须覆盖`start`方法。`start`方法启动小程序的执行。从`start`方法快速返回是一个良好的实践。如果需要执行计算密集型操作，最好为此目的启动一个新线程。

## `stop` 方法

大多数覆盖`start`方法的小程序也应该覆盖`stop`方法。`stop`方法应该暂停小程序的执行，这样当用户不查看小程序页面时，它就不会占用系统资源。例如，显示动画的小程序在用户不查看时应该停止尝试绘制动画。

## `destroy` 方法

许多小程序不需要覆盖`destroy`方法，因为它们的`stop`方法（在`destroy`之前调用）将执行关闭小程序执行所需的所有任务。但是，`destroy`方法适用于需要释放额外资源的小程序。

* * *

**注意：** 尽量保持`destroy`方法的实现尽可能简短，因为不能保证该方法会完全执行。在长`destroy`方法完成之前，Java 虚拟机可能会退出。

* * *
