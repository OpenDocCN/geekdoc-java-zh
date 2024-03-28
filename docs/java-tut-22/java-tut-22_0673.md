# 合成图形

> 原文：[`docs.oracle.com/javase/tutorial/2d/advanced/compositing.html`](https://docs.oracle.com/javase/tutorial/2d/advanced/compositing.html)

[`AlphaComposite`](https://docs.oracle.com/javase/8/docs/api/java/awt/AlphaComposite.html) 类封装了各种合成样式，确定重叠对象如何被渲染。`AlphaComposite` 还可以具有指定透明度的 alpha 值：alpha = 1.0 完全不透明，alpha = 0.0 完全透明（清除）。`AlphaComposite` 支持下表中显示的大多数标准 Porter-Duff 合成规则。

| 合成规则 | 描述 |
| --- | --- |
| 源覆盖 (`SRC_OVER`) ![源覆盖合成](img/85395aa80e20b02cc776d56ce31ed11e.png) | 如果正在渲染的对象（源）中的像素与先前渲染的像素（目标）位于相同位置，则源像素将覆盖目标像素。 |
| 源中 (`SRC_IN`) ![源中合成](img/e63ec0d05994afdcc5ae312604160f74.png) | 如果源和目标重叠，只有源区域的像素被渲染。 |
| 源减去 (`SRC_OUT`) ![源减去合成](img/ad0f2a651322e3cac55513d98ceed0f5.png) | 如果源和目标重叠，只有源区域外的像素被渲染。重叠区域的像素被清除。 |
| 目标覆盖 (`DST_OVER`) ![目标覆盖合成](img/11db5d580b448f1841945d150ea083c1.png) | 如果源和目标重叠，只有源区域外的像素被渲染。重叠区域的像素不会改变。 |
| 目标中 (`DST_IN`) ![目标中合成](img/1aa2993d7f2a286b6111b9215b3baedf.png) | 如果源和目标重叠，源的 alpha 值将应用于重叠区域的目标像素。如果 alpha = 1.0，则重叠区域的像素保持不变；如果 alpha 为 0.0，则清除重叠区域的像素。 |
| 目标减去 (`DST_OUT`) ![目标减去合成](img/e38722d93c4db177d0f119e6b60601bf.png) | 如果源和目标重叠，源的 alpha 值将应用于重叠区域的目标像素。如果 alpha = 1.0，则清除重叠区域的像素；如果 alpha 为 0.0，则重叠区域的像素保持不变。 |
| 清除 (`CLEAR`) ![清除重叠合成](img/3b797fb9c52e3bb8aa6bf59d5bd15e1e.png) | 如果源和目标重叠，清除重叠区域的像素。 |

要更改 [`Graphics2D`](https://docs.oracle.com/javase/8/docs/api/java/awt/Graphics2D.html) 类使用的合成样式，请创建一个 `AlphaComposite` 对象并将其传递给 `setComposite` 方法。

## 示例：合成

该程序演示了各种合成样式和 alpha 组合的效果。

<applet code="Composite" archive="examples/lib/CompositeApplet.jar" width="300" height="300" alt="applet showing compositing style and alpha combinations"><param name="permissions" value="sandbox"></applet>

* * *

**注意：** 如果您看不到 applet 运行，请至少安装[Java SE Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

``Composite.java`` 包含了此 applet 的完整代码。

通过调用 `AlphaComposite.getInstance` 并指定所需的合成规则来构造一个新的 `AlphaComposite` 对象 *ac*。

```java
AlphaComposite ac =
  AlphaComposite.getInstance(AlphaComposite.SRC);

```

当选择不同的合成规则或 alpha 值时，会再次调用 `AlphaComposite.getInstance`，并将新的 `AlphaComposite` 分配给 *ac*。所选的 alpha 值除了每个像素的 alpha 值外还会应用，并作为第二个参数传递给 `AlphaComposite`.`getInstance`。

```java
ac = AlphaComposite.getInstance(getRule(rule), alpha);

```

通过将 `AlphaComposite` 对象传递给 `Graphics 2D` 的 `setComposite` 方法来修改合成属性。对象被渲染到 `BufferedImage` 中，然后复制到屏幕上，因此合成属性被设置在 `BufferedImage` 的 `Graphics2D` 上下文中：

```java
BufferedImage buffImg = new BufferedImage(w, h,
                        BufferedImage.TYPE_INT_ARGB);
Graphics2D gbi = buffImg.createGraphics();
...
gbi.setComposite(ac);

```
