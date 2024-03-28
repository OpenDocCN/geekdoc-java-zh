# 控制渲染质量

> 原文：[`docs.oracle.com/javase/tutorial/2d/advanced/quality.html`](https://docs.oracle.com/javase/tutorial/2d/advanced/quality.html)

使用[`Graphics2D`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics2D.html)类的渲染提示属性来指定您是希望对象尽可能快地呈现还是您更喜欢呈现质量尽可能高。

要设置或更改`Graphics2D`上下文中的渲染提示属性，请构造一个[`RenderingHints`](https://docs.oracle.com/javase/8/docs/api/java/awt/RenderingHints.html)对象，并通过使用`setRenderingHints`方法将其传递给`Graphics2D`。如果您只想设置一个提示，可以调用`Graphics2D`的`setRenderingHint`并指定要设置的提示的键值对。（键值对在`RenderingHints`类中定义。）

例如，要设置抗锯齿首选项（如果可能的话），您可以使用`setRenderingHint`：

```java
public void paint (graphics g){
    Graphics2D g2 = (Graphics2D)g;
    RenderingHints rh = new RenderingHints(
             RenderingHints.KEY_TEXT_ANTIALIASING,
             RenderingHints.VALUE_TEXT_ANTIALIAS_ON);
    g2.setRenderingHints(rh);
...
}

```

* * *

**注意：**并非所有平台都支持修改渲染模式，因此指定渲染提示并不保证它们会被使用。

* * *

`RenderingHints`支持以下类型的提示：

| 提示 | 键 | 值 |
| --- | --- | --- |

| **抗锯齿** | `KEY_ANTIALIASING` | `VALUE_ANTIALIAS_ON` `VALUE_ANTIALIAS_OFF`

`VALUE_ANTIALIAS_DEFAULT` |

| **Alpha 插值** | `KEY_ALPHA_INTERPOLATION` | `VALUE_ALPHA_INTERPOLATION_QUALITY` `VALUE_ALPHA_INTERPOLATION_SPEED`

`VALUE_ALPHA_INTERPOLATION_DEFAULT` |

| **颜色渲染** | `KEY_COLOR_RENDERING` | `VALUE_COLOR_RENDER_QUALITY` `VALUE_COLOR_RENDER_SPEED`

`VALUE_COLOR_RENDER_DEFAULT` |

| **抖动** | `KEY_DITHERING` | `VALUE_DITHER_DISABLE` `VALUE_DITHER_ENABLE`

`VALUE_DITHER_DEFAULT` |

| **分数文本度量** | `KEY_FRACTIONALMETRICS` | `VALUE_FRACTIONALMETRICS_ON` `VALUE_FRACTIONALMETRICS_OFF`

`VALUE_FRACTIONALMETRICS_DEFAULT` |

| **图像插值** | `KEY_INTERPOLATION` | `VALUE_INTERPOLATION_BICUBIC` `VALUE_INTERPOLATION_BILINEAR`

`VALUE_INTERPOLATION_NEAREST_NEIGHBOR` |

| **渲染** | `KEY_RENDERING` | `VALUE_RENDER_QUALITY` `VALUE_RENDER_SPEED`

`VALUE_RENDER_DEFAULT` |

| **笔画规范化控制** | `KEY_STROKE_CONTROL` | `VALUE_STROKE_NORMALIZE` `VALUE_STROKE_DEFAULT`

`VALUE_STROKE_PURE` |

| **文本抗锯齿** | `KEY_TEXT_ANTIALIASING` | `VALUE_TEXT_ANTIALIAS_ON` `VALUE_TEXT_ANTIALIAS_OFF`

`VALUE_TEXT_ANTIALIAS_DEFAULT`

`VALUE_TEXT_ANTIALIAS_GASP`

`VALUE_TEXT_ANTIALIAS_LCD_HRGB`

`VALUE_TEXT_ANTIALIAS_LCD_HBGR`

`VALUE_TEXT_ANTIALIAS_LCD_VRGB`

`VALUE_TEXT_ANTIALIAS_LCD_VBGR`

|

| **LCD 文本对比度** | `KEY_TEXT_LCD_CONTRAST` | 值应为范围在 100 到 250 之间的正整数。较低的值（如 100）对应于在浅色背景上显示深色文本时更高的对比度。较高的值（如 200）对应于在浅色背景上显示深色文本时较低的对比度。一个典型有用的值在 140-180 的狭窄范围内。如果未指定值，则将应用系统或实现默认值。 |
| --- | --- | --- |

当提示设置为默认时，使用平台渲染默认值。
