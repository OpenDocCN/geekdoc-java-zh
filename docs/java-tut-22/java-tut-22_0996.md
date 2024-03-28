# 时间限制

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/timelimit.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/timelimit.html)

对搜索设置时间限制会对搜索操作等待答复的时间上限。当您不希望等待太长时间以获取答复时，这很有用。如果在搜索操作完成之前超过了指定的时间限制，则会抛出[`TimeLimitExceededException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/TimeLimitExceededException.html)。

要设置搜索的时间限制，请将毫秒数传递给[`SearchControls.setTimeLimit()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/SearchControls.html#setTimeLimit-int-)。以下`示例`将时间限制设置为 1 秒。

```java
// Set the search controls to limit the time to 1 second (1000 ms)
SearchControls ctls = new SearchControls();
ctls.setTimeLimit(1000);

```

要使此特定示例超过其时间限制，您需要重新配置它以使用要么是慢的服务器，要么是具有大量条目的服务器。或者，您可以使用其他策略使搜索时间超过 1 秒。

时间限制为零意味着没有设置时间限制，对目录的调用将无限期等待答复。
