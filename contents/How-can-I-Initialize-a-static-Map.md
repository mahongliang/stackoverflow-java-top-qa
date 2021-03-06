# 初始化静态map #

## 问题 ##

怎么在Java中初始化一个静态的map

**方法一**：静态初始化器

**方法二**：实例初始化（匿名子类）

下面是描述上面两种方法的例子

	import java.util.HashMap;
	import java.util.Map;
	public class Test{
		private static final Map<Integer, String> myMap = new HashMap<Integer, String>();
		static {
			myMap.put(1, "one");
			myMap.put(2, "two");
		}

		private static final Map<Integer, String> myMap2 = new HashMap<Integer, String>(){
			{
				put(1, "one");
				put(2, "two");
			}
		};
	}

## 答案 ##

### 答案1 ###

匿名子类初始化器是java的语法糖，我搞不明白为什么匿名类来初始化，而且如果create的类是final的话，它将不起作用

我创建固定大小图的时候使用static初始化器

	public class Test{
		private static final Map<Integer, String> myMap;
		static{
			Map<Integer, String> aMap = ...;
			aMap.put(1,"one");
			aMap.put(2,"two");
			myMap = Collections.unmodifiableMap(aMap);
		}
	}


### 答案2 ###

我喜欢用Guava（是 Collection 框架的增强和扩张）的方法初始化一个静态的，不可改变的map

	static fianl Map<Integer, String> myMap = ImmutablMap.of(
		1，"one",
		2, "two"
	)
·
当你的图entry的个数超过5个时候你就不能使用`ImmutableMap.of`可以试试`ImmutableMap.bulider()`

	static fianl Map<Integer, String> myMap = ImmutableMap.<Integer, String>builder()
	{
		.put(1, "one")
		.put(2, "two")
			
		.put(15, "fifteen")
		.build();
	}


# 原文链接 #

http://stackoverflow.com/questions/507602/how-can-i-initialize-a-static-map