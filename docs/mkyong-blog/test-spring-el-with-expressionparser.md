# 用 ExpressionParser 测试弹簧 el

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring3/test-spring-el-with-expressionparser/>

Spring expression language (SpEL)支持许多功能，您可以使用这个特殊的“ **ExpressionParser** ”接口来测试那些表达式特性。

这里有两个代码片段，展示了使用 Spring EL 的基本用法。

SpEL 来计算文字字符串表达式。

```java
 ExpressionParser parser = new SpelExpressionParser();
Expression exp = parser.parseExpression("'put spel expression here'");
String msg = exp.getValue(String.class); 
```

SpEL 来评估 bean 属性“item.name”。

```java
 Item item = new Item("mkyong", 100);
StandardEvaluationContext itemContext = new StandardEvaluationContext(item);

//display the value of item.name property
Expression exp = parser.parseExpression("name");
String msg = exp.getValue(itemContext, String.class); 
```

测试 SpEL 的几个例子。代码和注释应该是自我探索的。

```java
 import org.springframework.expression.Expression;
import org.springframework.expression.ExpressionParser;
import org.springframework.expression.spel.standard.SpelExpressionParser;
import org.springframework.expression.spel.support.StandardEvaluationContext;

public class App {
	public static void main(String[] args) {

		ExpressionParser parser = new SpelExpressionParser();

		//literal expressions 
		Expression exp = parser.parseExpression("'Hello World'");
		String msg1 = exp.getValue(String.class);
		System.out.println(msg1);

		//method invocation
		Expression exp2 = parser.parseExpression("'Hello World'.length()");  
		int msg2 = (Integer) exp2.getValue();
		System.out.println(msg2);

		//Mathematical operators
		Expression exp3 = parser.parseExpression("100 * 2");  
		int msg3 = (Integer) exp3.getValue();
		System.out.println(msg3);

		//create an item object
		Item item = new Item("mkyong", 100);
		//test EL with item object
		StandardEvaluationContext itemContext = new StandardEvaluationContext(item);

		//display the value of item.name property
		Expression exp4 = parser.parseExpression("name");
		String msg4 = exp4.getValue(itemContext, String.class);
		System.out.println(msg4);

		//test if item.name == 'mkyong'
		Expression exp5 = parser.parseExpression("name == 'mkyong'");
		boolean msg5 = exp5.getValue(itemContext, Boolean.class);
		System.out.println(msg5);

	}
} 
```

```java
 public class Item {

	private String name;

	private int qty;

	public Item(String name, int qty) {
		super();
		this.name = name;
		this.qty = qty;
	}

	//...
} 
```

输出

```java
 Hello World
11
200
mkyong
true 
```

**Note**
This article is demonstrates few basic usages of Spring expression parser, and you should visit this [official Spring expression documentation](http://web.archive.org/web/20190225101849/http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/expressions.html) for hundred of useful SpEL examples.

## 下载源代码

Download It – [Spring3-EL-Parser-Example.zip](http://web.archive.org/web/20190225101849/http://www.mkyong.com/wp-content/uploads/2011/06/Spring3-EL-Parser-Example.zip) (6 KB)[spring el](http://web.archive.org/web/20190225101849/http://www.mkyong.com/tag/spring-el/) [spring3](http://web.archive.org/web/20190225101849/http://www.mkyong.com/tag/spring3/)![](img/3515d679e000656e7d1e60603e598f67.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225101849/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="9273">







