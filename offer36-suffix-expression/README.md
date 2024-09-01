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
参考：https://liaoxuefeng.com/books/java/exception/index.html  
##### 理清try catch，throw, throws
try catch：捕获异常，并处理这个异常，不会让捕获到的异常导致程序中断</br>
throw：程序员手动抛出一个异常，这个异常后面的代码不会被执行，程序会找关于这个异常最近的try catch代码块，处理这个异常。  
注意：
**throw找到对应的try catch代码块后不会再回头执行执行throw后面的代码。** </br>
finally：不管程序有没有捕捉到try catch里面的异常，都一定会执行finally里面的程序。
##### 受检异常 非受检异常
## 时间空间复杂度
如果输入数组的长度是n，那么对其中的每个字符串都有一次push操作；  
如果是操作符，那么还需要进行数学计算和两次push操作。  
由于每个push操作、pop操作和数学计算都是O（1），因此总体时间复杂度是O（n）。由于栈中可能有O（n）个操作数，因此这种解法的空间复杂度也是O（n）。

