# 后缀表达式
## 题目：
后缀表达式是一种算术表达式，它的操作符在操作数的后面。输入一下用字符串数组表示的后缀表达式，请输出该后缀表达式的计算结果。假设输入的一定是有效的后缀表达式。  
例如，后缀表达式【“2”，“1”，“3”，“*”，“+”】对应的算术表达式是“2+1*3”，因此输出它的计算结果是5.
分析：后缀表达式又叫逆波兰式（Reverse Polish Notation，RPN）,是一种将操作符放在操作数后面的算术表达式。通常用的是中缀表达式，即操作符位于两个操作数中间，如“2+1\*3”。使用后缀表达式的好处是不需要使用括号。例如，中缀表达式的“2+1\*3”。后缀表达式不使用括号也能无歧义地表达这两个不同的算术表达式。  

### 后缀表达式【“2”，“1”，“3”，“*”，“+”】的计算过程  
1.从左到右扫描这个数组，首先遇到是的操作数“2”，由于这是后缀表达式，操作符还在后面。不知道操作符就不能计算，于是先将“2”保存到某个数据容器中。  
2.接下来的两个还是操作数，“1”和“3”，由于缺少操作符，因此还不知道如何计算，只好将它们先后保存到数据容器中。  
3.接下来遇到了一个操作符“*”。按照后缀表达式的规则，这个操作符对应的操作数是“1”和“3”，于是将它们从数据容器中取出来。此时容器在中先后保存的“2”、“1”和“3”这3个操作数，此时取出的是后保存的两个，最先保存的“2”仍然留在数据容器中。这看起来是“后入先出”的顺序，所以可以考虑用栈来实现这个数据容器。  
4.由于当前的操作符是“*”，因此将两个操作数“1”和“3”相乘，得到结果“3”。这个结果可能会成为后面操作符的操作数，因此仍然将它入栈。  
5.最后遇到的操作符“+”，此时栈中有两个操作数，即“2”和“3”，分别将它们出栈，然后计算工它们的和，得到”5“，再将结果”5“入栈。此时整个后缀表达式已经计算完毕，留在栈中的唯一的操作数”5“就是结果。  
| 步骤 | 输入| 操作 | 栈 | 注释 ｜
| :--- | :--- | :--- | :-- | :-- |
| 1  | "2" | 入栈 | [2] ||
| 2  | "1" | 入栈 | [2][1] | |
| 3  | "3" | 入栈 | [2][1][3] | |
| 4  | "*" | 乘法运算 | [2,3] | 3,1出栈，结果3入栈
| 5  | "+" | 加法运算 | [5] | 3,2出栈，结果5入栈


## ChatGPT提供的代码
```java
package offer36;

import java.util.Stack;

public class ChatGPT {
	// Method to evaluate a Reverse Polish Notation expression
	//评估逆波兰表达式的方法
	public int evalRPN(String[] tokens) {
		// Stack to store integers for the calculation
		//用于存储计算所需的整数的栈
		Stack<Integer> stack = new Stack<>();
		// Loop through each token in the input array
		//遍历tokens中的每个元素
		for (String token : tokens) {
			// Check if the token is an operator
			// 检查是否是一个操作符
			switch (token) {
				case "+":
				case "-":
				case "*":
				case "/":
					// Pop the top two elements from the stack for the operation
					// 从栈中弹出两个元素用于计算
					int num1 = stack.pop();
					int num2 = stack.pop();
					
					// Perform the calculation and push the result back to the stack
					// 将计算结果压入栈中
					stack.push(calculate(num2, num1, token));
					break;
				
				default:
					// If the token is a number, push it onto the stack
					// 如果遍历的token元素是一个数字，压入栈中
					stack.push(Integer.parseInt(token));
			}
		}
		// The final result will be the last element remaining on the stack
		//弹出栈中最后的元素
		return stack.pop();
	}
	
	// Helper method to perform the actual arithmetic operation
	// 辅助方法用来执行算术运算
	private Integer calculate(int num2, int num1, String operator) {
		// Perform the operation based on the operator provided
		// 基于提供的操作符执行计算
		switch(operator) {
			case "+":
				// Add the two numbers
				// 将两数想加
				return num2 + num1;
			case "-":
				// Subtract the second number from the first
				// 从第一个元素中减去第二个元素
				return num2 - num1;
			case "*":
				// Multiply the two numbers
				// 将两数相乘
				return num2 * num1;
			case "/":
				// Divide the first number by the second
				// 将两数相除
				return num2 / num1;
			default:
				// If an invalid operator is encountered, throw an exception
				// 如果遇到一个无效的操作符，抛异常
				throw new IllegalArgumentException("Invalid operator: " + operator);
		}
	}
}
```
### 知识点。
#### 复习一下switch  
https://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html  
好处就是很多个if else可以使代码看上去更简洁
#### 不懂的点：异常处理  
摘自：https://liaoxuefeng.com/books/java/exception/index.html  
程序运行的时候，经常会发生各种错误。  
比如，使用Excel的时候，它有时候会报错
##### Java的异常
在计算机程序运行的过程中，总是会出现各种各样的错误。  
有一些错误是用户造成的，比如，希望用户输入一个int类型的年龄，但是用记的输入是abc：  
```java
//假设用户输入了abc：
String s="abc";
int n=Integer.parseInt(s); //NumberFormatException!
```
程序想要读写某个文件的内容，但是用户已经把它删除了：
```
//用户删除了该文件
String t=readFile("C:\\abc.txt");//FileNotFound Exception!
```
还有一些错误是随机出现，并且永远不可能避免的。比如：
* 网络突然断了，连接不到远程服务器
* 内存耗尽，程序崩溃了；
* 用户点“打印”，但根本没有打印机；
* 。。。。
所以，一个健壮的程序必须处理各种各样的错误。  
所谓错误，就是程序调用某个函数的时候，如果失败了，就表示出错。  
调用方如何获知调用失败的信息？有两种方法：  
###### 方法一：约定返回错误码。
例如，处理一个文件，如果返回0，表示成功，返回其他整数，表示约定的错误码：
```java
int code = processFile("C:\\test.txt");
if(code==0){
//ok;
}else{
//error
switch(code){
	case 1:
	//file not found;
	case 2:
	//no read permission
	default:
	//unknown error;
	}
}
```
###### 方法二：在语言层面上提供一个异常处理机制  
**Java 内置了一套异常处理机制，总是使用异常来表示错误。**   
异常是一种class，因此它本身带有类型信息。异常可以在任何地方抛出，但只需要在上层捕获，这样就和方法调用分离了：
<img width="529" alt="Screenshot 2024-08-30 at 14 29 48" src="https://github.com/user-attachments/assets/821b23c3-b2b8-4573-95c1-707055ef197b">
####### Java内置的异常处理机制
Java内置的异常处理机制是指Java提供的一套用于处理程序运行时出现错误或异常情况的框架。通过使用异常处理机制，开发者可以编写更健壮的代码，从而避免程序在运行时因不可预见的错误而崩溃。
######## 异常分为三类：
######### 受检异常（Checked Exception）：
* 继承自“java.lang.Exception”类，但不包括继承自“RuntimeException”的异常
* 必须在代码中明确处理（使用try-catch块）或通过方法声明抛出（使用‘throws’关键字）
* 常见的受检异常包括‘IOException’，‘SQLException’等。
######### 非受检异常（Unchecked Excetpion）
* 继承自‘java.lang.RuntimeException’类
* 不需要在代码中显示处理，编译器不会强制要求捕获或声明。
* 常见的非受检异常包括‘NullPointerException’，“ArrayIndexOutofBoundsException”等。
######### 错误（Error）
* 继承自‘java.lang.Error’类
* 通常表示JVM自身遇到的问题，如内存不足（OutOfMemoryError）或栈溢出（StackOverflowError）
* 通常不建议在代码中捕获或处理‘Error’，因为它们表示严重的问题。
########## 异常处理的关键字
* try：定义了一个代码块，用于包围可能会抛出异常的代码。
* catch：紧跟在‘try’块之后，用于捕获特定类型的异常并处理它
* finally：块中的代码始终执行，无论是否抛出或捕获异常。
* throw：用于显式抛出一个异常。
* throws：用于方法声明中，表示该方法可能抛出的异常类型。
########### 基本的异常处理示例
```java
public class ExceptionExample {
    public static void main(String[] args) {
        try {
            int result = divide(10, 0);
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero!");
        } finally {
            System.out.println("Execution completed.");
        }
    }

    public static int divide(int a, int b) throws ArithmeticException {
        return a / b;
    }
}
```
##### 捕获异常
因为使用int类型的错误码，想要处理就非常麻烦。这种方式常见于底层C函数。
##### 抛出异常
##### 自定义异常
##### NullPointerException
##### 使用断言
##### 使用JDK Logging
##### 使用Log4j
##### 使用SLF4J和Logback



由于栈中只保存操作数，操作符不需要保存到栈中,因此上述代码创建的是一个整型栈。上述代码逐一扫描后缀表达式数组中的每个字符串。如果遇到的是一个操作数，则将其入栈；如果遇到的是一个操作符，则两个操作数出栈并执行相应的运算，然后将计算结果入栈。

## 时间空间复杂度
如果输入数组的长度是n，那么对其中的每个字符串都有一次push操作；  
如果是操作符，那么还需要进行数学计算和两次push操作。  
由于每个push操作、pop操作和数学计算都是O（1），因此总体时间复杂度是O（n）。由于栈中可能有O（n）个操作数，因此这种解法的空间复杂度也是O（n）。

