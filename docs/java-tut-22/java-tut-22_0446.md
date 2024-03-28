# 问题和练习：在容器内布置组件

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/QandE/questions-ch4.html`](https://docs.oracle.com/javase/tutorial/uiswing/QandE/questions-ch4.html)

## 问题

在以下每个问题中，选择最适合描述布局的布局管理器。假设由布局管理器控制的容器是`JPanel`。

1\. 该容器有一个组件应尽可能占据空间

| ![Layout1-1.png](img/b38bbad53f3494d668c27028b63d6b89.png) | ![Layout1-2.png](img/c5f368956cfdfa003d3c6efc4c084069.png) |
| --- | --- |

a. `BorderLayout`

b. `GridLayout`

c. `GridBagLayout`

d. a 和 b

e. b 和 c

2\. 该容器有一行组件，应该以相同大小显示，填充容器的整个区域。

![Layout2-1.png](img/dc674c959632ad95fb62ce0514ac81b8.png) ![Layout2-2.png](img/a1448c7ae98d8f50bf1fe8ae4c3f9d2b.png)

a. `FlowLayout`

b. `GridLayout`

c. `BoxLayout`

d. a 和 b

3\. 该容器以列形式显示多个组件，额外的空间位于前两个组件之间。

| ![Layout3-1.png](img/b9ef4ed676266bf873ea7854a25bbd48.png) | ![Layout3-2.png](img/dfcc51b23788f4775d64052fb84f537c.png) |
| --- | --- |

a. `FlowLayout`

b. `BoxLayout`

c. `GridLayout`

d. `BorderLayout`

4\. 该容器可以在不同时间显示三个完全不同的组件，可能取决于用户输入或程序状态。即使组件的大小不同，从一个组件切换到下一个也不应更改为组件分配的空间量。

![Layout4-1.png](img/e939e058e336515aaa4232bb3818bdaf.png) ![Layout4-2.png](img/021bc8948bc6bcd7cd54184a8bc8ee63.png)

a. `SpringLayout`

b. `BoxLayout`

c. `CardLayout`

d. `GridBagLayout`

## 练习

1\. 实现问题 1 中描述和显示的布局。

2\. 实现问题 2 中描述和显示的布局。

3\. 实现问题 3 中描述和显示的布局。

4\. 实现问题 4 中描述和显示的布局。

5\. 通过添加一行代码，使您为练习 2 编写的程序显示组件从右到左，而不是从左到右。

![Layout2-3.png](img/ceb14c230365902b0642b6816410884c.png) 检查您的答案。
