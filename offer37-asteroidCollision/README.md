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
....  
lambda总结  
Lambda表达式使Java编程更简洁、表达力更强，尤其是在处理函数式编程任务时。它提供了一种更简洁的方式来创建匿名方法，适用于回调、事件处理、集合操作、流处理等场景。通过Lambda表达式，你可以更方便地使用Java的函数式编程特性，从而提高代码的可读性和可维护性。  
### 自动装箱 自动拆箱
自动装箱（Autoboxing） 和 自动拆箱（Unboxing） 是Java语言中的两个重要特性，旨在简化基本数据类型（如int、char、double等）与其对应的封装类（如Integer、Character、Double等）之间的转换。  
自动装箱（Autoboxing）  
自动装箱 是指Java编译器自动将基本数据类型转换为其对应的封装类对象的过程。例如，将int类型转换为Integer对象。  
示例：  
```java
int a = 10;
Integer b = a;  // 自动装箱：int -> Integer
```
在这个例子中，a是一个int类型的基本数据类型。当我们将它赋值给Integer类型的变量b时，Java编译器会自动将a装箱为Integer对象。  
自动拆箱（Unboxing）  
自动拆箱 是指Java编译器自动将封装类对象转换为其对应的基本数据类型的过程。例如，将Integer对象转换为int类型。   
示例：  
```java
Integer b = 10;  // 自动装箱
int a = b;       // 自动拆箱：Integer -> int
```
在这个例子中，b是一个Integer对象。当我们将它赋值给int类型的变量a时，Java编译器会自动将b拆箱为int类型的值。  
为什么需要自动装箱和拆箱？  
在Java中，基本数据类型（如int、char等）效率高，但不能用于对象的场合。例如，集合类（如List、Map等）只能处理对象类型的数据，这时就需要将基本数据类型转换为其对应的封装类类型。自动装箱和拆箱特性使得这些转换更加简洁，无需手动进行显式的转换。  
自动装箱和拆箱的常见用法  
1.在集合类中使用：
* 由于Java的集合类只能存储对象类型，自动装箱允许你直接将基本数据类型添加到集合中。
```java
List<Integer> list = new ArrayList<>();
list.add(10);  // 自动装箱：int -> Integer
```
2.在条件判断中使用
* 自动拆箱使得在比较或运算时，可以直接使用封装类对象，而无需手动转换。
```java
Integer a = 100;
Integer b = 100;
if (a < b) {  // 自动拆箱：Integer -> int
    System.out.println("a is less than b");
}
```
3.在数学运算中使用  
*  自动拆箱允许在数学运算中混合使用基本数据类型和封装类。  
```java
Integer a = 5;
int b = 10;
int c = a + b;  // 自动拆箱：Integer -> int，然后进行加法运算
```
注意事项
* 性能影响：尽管自动装箱和拆箱简化了代码，但频繁使用可能导致性能问题，因为每次装箱都涉及创建新的对象，拆箱涉及解包操作。
* NullPointerException风险：在自动拆箱时，如果封装类型对象为null，会抛出NullPointerException。
```java
Integer a = null;
int b = a;  // 抛出 NullPointerException
```
总结  
自动装箱和拆箱大大简化了基本数据类型与对象类型之前的转换，提升了代码的简洁和可读性。然后，在使用时应注意可能的性能问题和‘NullPointerException’的风险
### 为什么不直接返回Stack呢
在Java中，不能直接返回 Stack 而是要将其转换成数组，通常是出于以下几个原因：  
1. 返回类型的一致性   
*  如果方法的签名指定了返回一个数组（如 int[]），则必须返回该类型的数据。如果直接返回 Stack<Integer>，编译器会报错，因为 Stack<Integer> 与 int[] 是不同的类型。  
*  例如，如果你的方法签名是`public int[] asteroidCollision(int[] asteroids)`,则必须返回`int[]`,而不能返回`Stack<Integer>`.  
2. 数据的适当封装  
* 在很多情况下，方法调用都可能只需要处理结果数组而不需要知道内部你使用了什么数据结构（如‘Stack’）。返回数组比返回‘Stack’更符合信息隐藏的原则，也让调用者更方便使用返回的数据  
3.标准化接口  
*  数组是一种更基础的数据结构，使用更广泛。在很多场景中，特别是在API设计中，返回数组通常比返回‘Stack’或其他集合类型更合适，因为数组可以直接在Java代码中使用、操作和遍历，而不需要额外的处理  
*  比如，数组可以直接用于‘for’循环和‘for each’循环，而‘Stack’需要先进行遍历  
4.防止外部个性
* 如果返回‘Stack’对象，外部代码可以直接修改这个‘Stack’，这可能导致未预期的结果或错误，如果转换成数组，外部代码只能读取数据，无法直接修改原来的栈结构，增强了安全性。
```java
public int[] asteroidCollision(int[] asteroids) {
    Stack<Integer> stack = new Stack<>();
    // ... 处理逻辑
    // 将 Stack 转换为数组返回
    return stack.stream().mapToInt(i -> i).toArray();
}
```
总结：
将‘Stack’转换为数组并返回是一种符合Java惯用法的做法，确保返回的数据类型符合方法签名，保持数据封装的完整性，并且避免外部代码对内部数据结构的直接修改。





