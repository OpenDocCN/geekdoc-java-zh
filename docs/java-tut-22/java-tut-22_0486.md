# 如何编写更改监听器

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/events/changelistener.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/changelistener.html)

更改监听器类似于属性更改监听器。更改监听器在一个对象上注册 —— 通常是一个组件，但也可以是另一个对象，比如一个模型 —— 当对象发生更改时，监听器会收到通知。与属性更改监听器的主要区别在于，更改监听器不会通知*什么*发生了变化，而只是通知源对象*发生了*变化。因此，当只需要知道对象以任何方式发生了变化时，更改监听器是最有用的。

几个 Swing 组件（包括 JTabbedPane、JViewPort）依赖更改事件进行基本功能 —— 滑块、颜色选择器和微调器。要了解滑块中的值何时更改，需要注册更改监听器。同样，需要在颜色选择器上注册更改监听器，以便在用户选择新颜色时获得通知。您需要在微调器上注册更改监听器，以便在微调器的值更改时收到通知。

这是一个滑块更改事件处理代码的示例：

```java
*//...where initialization occurs:*
framesPerSecond.addChangeListener(new SliderListener());
...
class SliderListener implements ChangeListener {
    public void stateChanged(ChangeEvent e) {
        JSlider source = (JSlider)e.getSource();
        if (!source.getValueIsAdjusting()) {
            int fps = (int)source.getValue();
            ...
        }    
    }
}

```

你可以在使用 Swing 组件示例索引中找到`SliderDemo`的源文件。

## 更改监听器 API

ChangeListener 接口

*因为`ChangeListener`只有一个方法，所以它没有对应的适配器类。*

| 方法 | 目的 |
| --- | --- |
| [stateChanged(ChangeEvent)](https://docs.oracle.com/javase/8/docs/api/javax/swing/event/ChangeListener.html#stateChanged-javax.swing.event.ChangeEvent-) | 当被监听组件改变状态时调用。 |

ChangeEvent 类

| 方法 | 目的 |
| --- | --- |
| [Object getSource()](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html#getSource--) (*在 `java.util.EventObject` 中*) | 返回触发事件的对象。 |

## 使用更改监听器的示例

以下表格列出了使用更改监听器的示例。

| 示例 | 描述位置 | 注释 |
| --- | --- | --- |
| `SliderDemo` 和 `SliderDemo2` | 如何使用滑块 | 在控制动画速度的滑块上注册一个更改监听器。更改监听器会忽略更改事件，直到用户释放滑块。 |
| `ColorChooserDemo` 和 `ColorChooserDemo2` | 如何使用颜色选择器 | 在颜色选择器的选择模型上使用更改监听器，以便在用户更改当前颜色时获得通知。 |
| `SpinnerDemo3` | 检测 Spinner 值的变化 在 如何使用 Spinners 中。 | 使用一个变化监听器在日期字段 Spinner 上改变文本颜色，当 Spinner 的日期发生变化时。 |
| `SpinnerDemo4` | 检测 Spinner 值的变化 在 如何使用 Spinners 中。 | 使用一个变化监听器在 Spinner 上循环灰度，当 Spinner 的值发生变化时。 |

| `ConverterRangeModel` 及其子类，

`FollowerRangeModel` | 如何使用模型 | 为 `Converter` 演示中使用的滑块实现自定义模型。两个模型在必要时都明确触发变化事件。
