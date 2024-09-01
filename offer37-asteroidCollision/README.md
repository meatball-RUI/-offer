# 小行星相撞
## 题目
输入一个表示小行星的数组，数组中每个数字的绝对值表示小行星的大小，数字的正负号表示小行星运动的方向，正号表示向右飞行，负号表示向左飞行。如果两颗小行星相撞，那么体积较小的小行星将会爆炸最终消失，体积较大的小行星不受影响。如果相撞的两颗小行星大小相同，那么它们都会爆炸消失。飞行方向相同的小行星永远不会相撞。求最终剩下的小行星。  
eg  
有6颗小行星【4，5，-6，4，8，-5】,它们相撞之后最终剩下3颗小行星【-6，4，8】  
## 问题描述
给定一个整数数组‘asteroids’，每个整数代表一个小行星：  
* 正数表示小行星向右移动  
* 负数表示小行星向左移动

当两个小行星相遇（一个向右，一个向左），它们会发生碰撞  
* 如果两个小行星大小相同，它们都会消失。
* 如果两个小行星大小不同，较小的小行星会消失，较大的小行星会继续移动  
如果没有小行星碰撞，则小行星会继续向它们原本的方向移动
## 小行星相撞过程表
| 步骤 | 小行星| 操作 | 栈 | 注释 
| :--- | :--- | :--- | :-- | :-- 
| 1  | 4 | 入栈 | [4] | 在它之前没有小行星，所以入栈 
| 2  | 5 | 入栈 | [4][5] | 跟4的方向相同，不会相撞，所以入栈 
| 3  | -6 | 相撞 | [-6] | 因为跟前面的小行星是对着的，所以-6 5相撞，5出栈，4，-6相撞，4出栈，-6入栈 
| 4  | 4 | 入栈 | [-6，4] | 方向相反（一个往左一个往右），不会相撞，入栈
| 5  | 8 | 入栈 | [-6，4，8] | 方向相同，入栈
| 6 | -5 | 相撞 | [-6，4，8] | 方向相对，8跟-5相撞，-5消失

## ChatGPT code加自己理解的comments
```java
package offer37;

import java.util.Stack;

public class OfferAddComments {
	public int[] asteroidCollision(int[] asteroids) {
		//创建一个栈用来存储还未销毁的小行星
		Stack<Integer> stack=new Stack<>();
		//遍历所有小行星
		for(int as:asteroids) {
			//当栈不为空时，并且栈顶小行星向右，当前小行向左，如果栈顶小行星小于当前小行星，栈顶小行星销毁
			//用while循环看是否要继续销毁栈顶小行星
			while(!stack.isEmpty()&&stack.peek()>0&&stack.peek()<-as) {
				stack.pop();
			}
			//当栈不为空时，并且栈顶小行星向右，当前小行星向左，两小行星的绝对值相等，
			//当前小行星跟栈顶小行星都销毁，此时不会再循环了，因为当前小行星已经被销毁
			//不能再跟栈内的小行星继续比较了
			if(!stack.isEmpty()&&as<0&&stack.peek()==-as) {
				stack.pop();
			}
			//否则如果当前栈为空或栈顶小行星向左或当前小行星向右
			//此时栈顶小行星向左时，就确保了不管当前小行星向左还是向右都不会相撞，可以压入栈中
			//同理当前小行星向右时，就确保了不管当前小行星向左还是向右都不会相撞，可以压入栈中
			else if(as>0 || stack.empty()||stack.peek()<0) {
				stack.push(as);//当前小行星入栈
			}
		}
		//将栈中的元素转换成int数组来返回
		return stack.stream().mapToInt(i -> i).toArray();
	}
}
```
## 知识瓶井区
### 不理解return stack.stream().mapToInt(i -> i).toArray();这行代码。  
逐步解析：  
1.`stack.stream`:
将栈中的元素转化为一个流('Stream<Integer>')。流是一种高级迭代器，可以用来执行各种操作，比如过滤、映射、归约等。
2.`mapToInt(i->i)`:  
* 这个方法是一个中间操作，它将流中的元素映射（转换）为另一种形式。  
* mapToInt(i -> i) 表示将流中的每一个 Integer 元素 i 映射为对应的 int 基本类型。  
* i -> i 是一个 Lambda 表达式，表示对于流中的每个元素 i，返回它本身。这个表达式的作用是直接将 Integer 转换为 int。  
3.toArray():
* 这个方法是流的终端操作，表示将流中的元素收集到一个数组中。
* 在这里，toArray() 将 IntStream 转换为一个 int[] 数组
### 不理解Lambda
在Java中，Lambda表达式是一种简洁的函数式编程方式，用于表示一个匿名函数。它于Java 8引入，目的是简化代码、增强可读性，并使函数式编程更容易实现。  
#### Lambda表达式的语法
Lambda表达式的基本语法是:  
```java
(parameters) -> expression
```
或者，如果有多个语句:  
```
(parameters) -> {
    // statements
}
```
主要组成部分   
1.参数列表：  
* 括号内的参数列表，可以是零个、一个或多个参数。如果只有一个参数，可以省略括号。  
* 例如：x -> x * x（一个参数），(x, y) -> x + y（多个参数），() -> System.out.println("Hello")（没有参数）
2.箭头符号 ->：  
* 用于分隔参数列表和表达式体。
3.表达式体：
* 可以是单一的表达式或包含多个语句的代码块
* 单一表达式：自动返回值。
* 代码块：需要使用`return`语句来返回值
示例：  
1.简单的Lambda表达式  
```java
// Lambda 表达式：一个参数，返回它的平方
Function<Integer, Integer> square = x -> x * x;
System.out.println(square.apply(5)); // 输出: 25
```
2.多个参数和代码块  
```java
// Lambda 表达式：两个参数，返回它们的和
BinaryOperator<Integer> add = (x, y) -> {
    int result = x + y;
    return result;
};
System.out.println(add.apply(5, 3)); // 输出: 8
```
3.没有参数的Lambda表达式
```java
// Lambda 表达式：无参数，打印一条消息
Runnable greet = () -> System.out.println("Hello, world!");
greet.run(); // 输出: Hello, world!
```






