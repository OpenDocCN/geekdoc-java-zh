# 方法命名约定

> 原文：[`docs.oracle.com/javase/tutorial/datetime/overview/naming.html`](https://docs.oracle.com/javase/tutorial/datetime/overview/naming.html)

日期时间 API 提供了丰富的方法集合，涵盖了丰富的类。在可能的情况下，方法名称在类之间保持一致。例如，许多类提供了一个`now`方法，用于捕获与该类相关的当前时刻的日期或时间值。还有`from`方法允许从一个类转换为另一个类。

方法名称前缀也有标准化。由于日期时间 API 中的大多数类都是不可变的，API 不包括`set`方法。（创建后，不可变对象的值不能更改。`set`方法的不可变等效方法是`with`。）以下表列出了常用的前缀：

| 前缀 | 方法类型 | 用途 |
| --- | --- | --- |
| `of` | 静态工厂 | 创建一个实例，其中工厂主要验证输入参数，而不是转换它们。 |
| `from` | 静态工厂 | 将输入参数转换为目标类的实例，这可能涉及从输入中丢失信息。 |
| `parse` | 静态工厂 | 解析输入字符串以生成目标类的实例。 |
| `format` | 实例 | 使用指定的格式化程序格式化时间对象中的值以生成字符串。 |
| `get` | 实例 | 返回目标对象的状态的一部分。 |
| `is` | 实例 | 查询目标对象的状态。 |
| `with` | 实例 | 返回目标对象的副本，其中一个元素已更改；这相当于 JavaBean 上的`set`方法的不可变版本。 |
| `plus` | 实例 | 返回目标对象的副本，其中添加了一定数量的时间。 |
| `minus` | 实例 | 返回目标对象的副本，其中减去了一定数量的时间。 |
| `to` | 实例 | 将此对象转换为另一种类型。 |
| `at` | 实例 | 将此对象与另一个对象组合。 |
