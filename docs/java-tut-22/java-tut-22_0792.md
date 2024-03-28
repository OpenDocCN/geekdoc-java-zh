# JDK 中的属性设置

> 原文：[`docs.oracle.com/javase/tutorial/jaxp/properties/propSettings.html`](https://docs.oracle.com/javase/tutorial/jaxp/properties/propSettings.html)

以下表格显示了 JDK 中新属性的默认值和行为。

| 访问属性的值 | 默认值 | 设置 FSP(a) | jaxp.properties | 系统属性 | API 属性 |
| --- | --- | --- | --- | --- | --- |
| 7u40 | `all` | 无更改 | 覆盖 | 覆盖 | 覆盖 |
| JDK8 | `all` | 更改为 "" | 覆盖 | 覆盖 | 覆盖 |

(a) 设置 FSP 意味着明确使用 JAXP 工厂的 `setFeature` 方法设置 FEATURE_SECURE_PROCESSING。

(b) 7u40 和 JDK8 之间唯一的行为差异是，在 7u40 中设置 FSP 不会更改 `accessExternal*` 属性，但在 JDK8 中会将值设置为空字符串。在 JDK8 中，设置 FSP 被视为选择加入。

(c) 表中从左到右的顺序反映了覆盖顺序。例如，如果通过 API 设置了 `accessExternal` 属性，则会覆盖其他可能已通过其他方式设置的属性。
