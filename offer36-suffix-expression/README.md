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
########### 运行时行为
* try 块中的代码首先被执行。
* 如果在 try 块中抛出异常，控制权将立即转移到与该异常类型匹配的**catch 块**。
* 如果异常在 try 块中抛出但没有匹配的 catch 块，异常将向上传播到调用者。
* finally 块中的代码始终执行，无论是否抛出或捕获异常。
######### 自定义异常
开发者还可以通过继承 Exception 或 RuntimeException 类来创建自定义异常。自定义异常通常用于表示应用程序中特定的错误情况。
```java
class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}
```
总结来说，Java 的异常处理机制通过 try-catch-finally 和 throw-throws 关键字，帮助开发者更好地管理和处理运行时错误，确保程序的健壮性和可维护性。  

######### 举例受检异常 非受检异常
以下是 Java 中受检异常和非受检异常的示例：
1. 受检异常（Checked Exception）示例. 
受检异常是那些在编译时被强制要求处理的异常。常见的受检异常包括 IOException、SQLException 等。  
示例：处理文件输入输出的 IOException
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class CheckedExceptionExample {
    public static void main(String[] args) {
        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader("example.txt"));
            String line = reader.readLine();
            System.out.println(line);
        } catch (IOException e) {
            System.out.println("An I/O error occurred: " + e.getMessage());
        } finally {
            try {
                if (reader != null) {
                    reader.close();
                }
            } catch (IOException e) {
                System.out.println("Failed to close the reader: " + e.getMessage());
            }
        }
    }
}
```
解释：  

    * FileReader 和 BufferedReader 是处理文件输入输出的类，可能会抛出 IOException。
    * 因为 IOException 是受检异常，必须要么通过 try-catch 块捕获，要么在方法签名中声明抛出。
2. 非受检异常（Unchecked Exception）示例   
	非受检异常是那些在编译时不强制要求处理的异常。它们主要包括继承自 RuntimeException 的异常，如 NullPointerException、ArrayIndexOutOfBoundsException 等。

示例：数组越界的 ArrayIndexOutOfBoundsException
```java
public class UncheckedExceptionExample {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3};
        try {
            System.out.println(numbers[3]);  // 访问越界元素
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Array index out of bounds: " + e.getMessage());
        }
    }
}
```
解释：
    * 访问数组 numbers 中的第 4 个元素（索引为 3），但数组实际只有 3 个元素（索引为 0, 1, 2）。
    * 这会抛出 ArrayIndexOutOfBoundsException，这是非受检异常，因此编译器不会强制要求捕获或处理。

总结
    * 受检异常：例如 IOException，必须被捕获或在方法签名中声明，否则编译器会报错。
    * 非受检异常：例如 ArrayIndexOutOfBoundsException，编译器不强制要求处理，但仍可以使用 try-catch 来捕获处理
######### 显示异常 隐式异常
在 Java 异常处理中，“显示异常”和“隐式异常”并不是标准的术语，但我们可以通过上下文理解它们的大致含义。
显示异常（Explicit Exception）  

显示异常通常是指由程序员显式抛出的异常。程序员使用 throw 关键字在代码中手动抛出一个异常，这种异常是“显示的”，因为它直接体现在代码中。  

示例：显示抛出自定义异常  
```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

public class ExplicitExceptionExample {
    public static void main(String[] args) {
        try {
            checkAge(15);
        } catch (InvalidAgeException e) {
            System.out.println("Caught Exception: " + e.getMessage());
        }
    }

    public static void checkAge(int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("Age must be 18 or older.");
        }
    }
}
```
解释：  

    在 checkAge 方法中，程序员使用 throw 关键字显式地抛出了 InvalidAgeException。  
    这种方式使得异常的抛出是明确的、可见的，程序员决定何时何地抛出异常。  
隐式异常（Implicit Exception）  

隐式异常通常是指程序运行过程中，由 Java 虚拟机（JVM）自动抛出的异常。程序员并没有在代码中显式地抛出这些异常，它们是在运行时由 JVM 根据程序的状态自动抛出的。  

示例：隐式抛出的空指针异常（NullPointerException）  
```java
public class ImplicitExceptionExample {
    public static void main(String[] args) {
        String str = null;
        try {
            System.out.println(str.length());  // 试图访问空对象的方法
        } catch (NullPointerException e) {
            System.out.println("Caught Exception: " + e.getMessage());
        }
    }
}
```
解释：  

    这里 str 是一个 null 引用，试图调用 str.length() 会导致 NullPointerException。  
    这个异常是由 JVM 在运行时自动抛出的，程序员并没有显式地用 throw 抛出它。  

总结    

    * 显示异常（Explicit Exception）：由程序员在代码中显式地使用 throw 关键字抛出的异常。  
    * 隐式异常（Implicit Exception）：由 JVM 在程序运行时根据具体情况自动抛出的异常，程序员没有显式地抛出这些异常。  
尽管这两种异常的术语不常见，它们的概念对于理解异常处理的不同方式是有帮助的。  

######### throw throws区别  
throw 和 throws 是 Java 中用于异常处理的两个关键字，但它们有不同的用途和语法位置。下面详细介绍它们的区别：
1. throw 关键字  
* 用途: throw 关键字用于显式地抛出一个异常。在代码执行过程中，当遇到某种特定情况时，可以使用 throw 抛出一个异常对象，立即终止当前方法的执行并将控制权交给调用方，或者被本方法内部的 catch 块捕获。
* 语法位置: throw 关键字出现在方法体内，紧跟在它后面的必须是一个异常对象。
* 用法示例:
```java
public class ThrowExample {
    public static void main(String[] args) {
        try {
            validateAge(15);
        } catch (IllegalArgumentException e) {
            System.out.println("Caught Exception: " + e.getMessage());
        }
    }

    public static void validateAge(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("Age must be 18 or older.");  // 显式抛出异常
        }
    }
}
```
解释: 在 validateAge 方法中，当 age 小于 18 时，throw 关键字显式抛出 IllegalArgumentException，并将控制权交给调用方。  
2. throws 关键字  
用途: throws 关键字用于声明一个方法可能会抛出的异常类型。它告诉调用该方法的代码，这些异常有可能在方法执行时被抛出，因此调用者需要处理这些异常（通过 try-catch 处理或在自己的方法签名中继续声明 throws）。  
语法位置: throws 关键字出现在方法声明的末尾，后面跟随一个或多个异常类型，用逗号分隔。  
用法示例:  
```java
import java.io.IOException;

public class ThrowsExample {
    public static void main(String[] args) {
        try {
            readFile("example.txt");
        } catch (IOException e) {
            System.out.println("Caught Exception: " + e.getMessage());
        }
    }

    public static void readFile(String fileName) throws IOException {  // 声明可能抛出的异常
        // 模拟可能抛出 IOException 的操作
        throw new IOException("File not found");
    }
}
```
解释: readFile 方法在声明中使用了 throws IOException，表示该方法可能会抛出 IOException。调用该方法的代码必须处理这个可能的异常。  
3. 总结  
    throw:  
        用于在方法内部显式抛出一个异常。  
        需要在代码中创建并抛出一个异常对象。  
        立即终止当前代码的执行。  
    throws:  
        用于在方法声明中表示该方法可能会抛出的异常类型。  
        允许调用者知道哪些异常需要处理。  
        不会直接抛出异常，而是提供一个异常声明。  
两者结合使用时，throw 是实际抛出异常的操作，而 throws 是声明方法可能抛出的异常。

######### throws再多一些例子用来理解
为了更好地理解 throws 关键字的用法，下面提供一些具体的示例，这些示例涵盖了不同的场景，展示了 throws 如何用于声明方法可能抛出的异常类型。  
示例 1: 声明单个受检异常  

假设我们有一个读取文件的函数，它可能会抛出 IOException。  

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

