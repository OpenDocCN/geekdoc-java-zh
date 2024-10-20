# Java 8–string joiner 示例

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-stringjoiner-example/>

在这篇文章中，我们将向你展示一些连接字符串的例子。

## 1.弦乐演奏者

1.1 通过分隔符连接字符串

```java
 StringJoiner sj = new StringJoiner(",");
        sj.add("aaa");
        sj.add("bbb");
        sj.add("ccc");
        String result = sj.toString(); //aaa,bbb,ccc 
```

1.2 通过分隔符连接字符串，以提供的前缀开始，以提供的后缀结束。

```java
 StringJoiner sj = new StringJoiner("/", "prefix-", "-suffix");
        sj.add("2016");
        sj.add("02");
        sj.add("26");
        String result = sj.toString(); //prefix-2016/02/26-suffix 
```

## 2.字符串连接

StringJoiner 由 static `String.join()`内部使用。

2.1 用分隔符连接字符串。

```java
 //2015-10-31
	String result = String.join("-", "2015", "10", "31" ); 
```

2.2 用分隔符连接一个列表<string>。

```java
 List<String> list = Arrays.asList("java", "python", "nodejs", "ruby");
 	//java, python, nodejs, ruby
	String result = String.join(", ", list); 
```

## 3.收集器.加入

两个`Stream`和`Collectors.joining`的例子。

3.1 加入`List<String>`示例。

```java
 List<String> list = Arrays.asList("java", "python", "nodejs", "ruby");

	//java | python | nodejs | ruby
	String result = list.stream().map(x -> x).collect(Collectors.joining(" | ")); 
```

3.2 连接列表`<Object>`示例。

```java
 void test(){

        List<Game> list = Arrays.asList(
                new Game("Dragon Blaze", 5),
                new Game("Angry Bird", 5),
                new Game("Candy Crush", 5)
        );

        //{Dragon Blaze, Angry Bird, Candy Crush}
        String result = list.stream().map(x -> x.getName())
			.collect(Collectors.joining(", ", "{", "}"));

    }

    class Game{
        String name;
        int ranking;

        public Game(String name, int ranking) {
            this.name = name;
            this.ranking = ranking;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getRanking() {
            return ranking;
        }

        public void setRanking(int ranking) {
            this.ranking = ranking;
        }
    } 
```

## 参考

1.  [Stream–collectors . joining JavaDoc](http://web.archive.org/web/20210815051317/https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#joining-java.lang.CharSequence-)
2.  [StringJoiner JavaDoc](http://web.archive.org/web/20210815051317/https://docs.oracle.com/javase/8/docs/api/java/util/StringJoiner.html)

Tags : [java8](http://web.archive.org/web/20210815051317/https://mkyong.com/tag/java8/)<input type="hidden" id="mkyong-current-postId" value="13948">